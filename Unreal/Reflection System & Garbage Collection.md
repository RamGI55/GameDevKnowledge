
As long as you use C++, a low-level language can manage the memory, memory management is the crucial in many degree.
Unreal Engine has in built memory management system which keep clear the memory. 

#### Reflection System 

Reflection system is the self-examination system runs in the runtime. Ability of the program understanding structure and the metadata.  `UObject` is the key point of the reflection system, defines a templet for the new object or actor class. 

#### `UObject` Handling 

Marking the classes, properties and function with the macros, gives unreal engine access to them and makes it allow under-the-hood handling features to implanted
`UObject` Handling needed for: 
- **Garbage Collection** 
- **Automatic Property Init** 
- **Auto Update of References** 
- **Serialisation** 
- **Editor Integrations** 

#### Property and Function Specifiers 

You can check all property and function specifiers link below: 
https://dev.epicgames.com/documentation/en-us/unreal-engine/property-specifiers?application_version=4.27
https://dev.epicgames.com/documentation/en-us/unreal-engine/ufunctions-in-unreal-engine

#### Garbage Collectors

Like Java and C#, Unreal C++ has in built automatic memory management system called `garbage collectors` which allows automatically clear the memory once the instance is no longer valid or exist. The `Garbage collection` system runs automatically in 60 secs (in default). It is looking the reflection system to look at the properties of the object and follow the references. Hence, object must be under the reflection system to garbage collection system works. Instancing as `UObject` is the one of the methods. 

Garbage Collection under-the-hood 
https://forums.unrealengine.com/t/knowledge-base-garbage-collector-internals/501800/6

```cpp 
UPROPERTY()
float Foo;    // Garbarge Collector can collect this 

float foo2;   // Cannot collect
```

In the GarbageCollection.cpp, `ContainsObjectReference()` method returns `true` if the property contains the `UObject` reference. 

```cpp 
 // In GarbageCollection.cpp
 bool FProperty::ContainsObjectReference(TArray<const FStructProperty*>& EncounteredStructProps, EPropertyObjectReferenceType InReferenceType) const  
{  
    return false;  
}

// array, maps and other type of the data follows

```

`ContainObjectReference()` method also implied to the other type of the data include the containers and `UStruct`. 

#### How Garbage Collector Works In Unreal Engine

##### Core GC Patterns 
Unreal Engine's garbage collection process follows these main phases: 
1. **Mark the Phase (Reachability Phases)**
	-  All objects are initially marked as `MaybeUnreachable` Object 
	- The GC traverse and reaches to the marked object as `Reachable` started from root objects. 
	- Using Object flag system for efficient traversal, which divides 3 states for the reachability; `Reachable`, `Unreachable` , and  `MaybeUnreachable` .
	
```cpp
// Part of the GarbageCollection.cpp 

EInternalObjectFlags GReachableObjectFlag = EInternalObjectFlags::ReachabilityFlag0; 

EInternalObjectFlags GUnreachableObjectFlag = EInternalObjectFlags::ReachabilityFlag1;

EInternalObjectFlags GMaybeUnreachableObjectFlag = EInternalObjectFlags::ReachabilityFlag2;

```

As the code snippet, unreal engine defines three type of the object based on the reachability and optimise the traversal logic, only works to the unreachable objects. 


2. **Sweep Phase (Object Destruction)**
	- Collect and destroy `Unreachable` objects, with calling `ConditionalBeginDestroy()` and `ConditionalFinishDestory()` 


 ``` cpp 
// Part of the GarbageCollection.cpp 
template<bool bPerformFullPurge>  
void CollectGarbageImpl(EObjectFlags KeepFlags)  
{  
    {  
       // Reachability analysis.  
       {  
          const EGCOptions Options = GetReferenceCollectorOptions(bPerformFullPurge);  
  
          // Perform reachability analysis.  
          FRealtimeGC GC;  
          GC.PerformReachabilityAnalysis(KeepFlags, Options);  
       }  
    }  
}  
  
template<bool bPerformFullPurge>  
void PostCollectGarbageImpl(EObjectFlags KeepFlags)  
{  
    const double PostCollectStartTime = FPlatformTime::Seconds();  
  
    using namespace UE::GC;  
    using namespace UE::GC::Private;     
  
    if (!GIsIncrementalReachabilityPending)  
    {  
       FContextPoolScope ContextPool;  
       TConstArrayView<TUniquePtr<FWorkerContext>> AllContexts = ContextPool.PeekFree();  
       // This needs to happen before clusters get dissolved otherwisise cluster information will be missing from history  
       UpdateGCHistory(AllContexts);  
  
       // Reconstruct clusters if needed  
       if (GUObjectClusters.ClustersNeedDissolving())  
       {  
          const double StartTime = FPlatformTime::Seconds();  
          GUObjectClusters.DissolveClusters();  
          UE_LOG(LogGarbage, Log, TEXT("%f ms for dissolving GC clusters"), (FPlatformTime::Seconds() - StartTime) * 1000);  
       }  
  
       DumpGarbageReferencers(AllContexts);  
  
       Swap(GUnreachableObjectFlag, GMaybeUnreachableObjectFlag);  
  
       const EGatherOptions GatherOptions = GetObjectGatherOptions();  
       DissolveUnreachableClusters(GatherOptions);  
  
       // This needs to happen after DissolveUnreachableClusters since it can mark more objects as unreachable  
       if (GReachabilityState.GetNumIterations() > 1)  
       {  
          ClearWeakReferences<true>(AllContexts);  
       }  
       else  
       {  
          ClearWeakReferences<false>(AllContexts);  
       }  
  
       StopVerseGC();  
  
       if (bPerformFullPurge)  
       {  
          ContextPool.Cleanup();  
       }  
  
       GGatherUnreachableObjectsState.Init();  
  
       if (bPerformFullPurge || !GAllowIncrementalGather)  
       {  
          GatherUnreachableObjects(GatherOptions, /*TimeLimit =*/ 0.0);  
       }  
    }  
  
    GIsGarbageCollectingAndLockingUObjectHashTables = false;  
    UnlockUObjectHashTables();  
  
    GIsGarbageCollecting = false;  
  
    // The hash tables lock was released when reachability analysis was done.  
    // BeginDestroy, FinishDestroy, destructors and callbacks are allowed to call functions like StaticAllocateObject and StaticFindObject.    // Now release the GC lock to allow async loading and other threads to perform UObject operations under the FGCScopeGuard.    ReleaseGCLock();  
  
    if (!GIsIncrementalReachabilityPending)  
    {  
       // Fire post-reachability analysis hooks  
       {  
          TRACE_CPUPROFILER_EVENT_SCOPE(BroadcastPostReachabilityAnalysis);  
          FCoreUObjectDelegates::PostReachabilityAnalysis.Broadcast();  
       }  
  
       if (bPerformFullPurge || !GIncrementalBeginDestroyEnabled)  
       {  
          UnhashUnreachableObjects(/**bUseTimeLimit = */ false);  
          FScopedCBDProfile::DumpProfile();  
       }  
  
       // Set flag to indicate that we are relying on a purge to be performed.  
       GObjPurgeIsRequired = true;  
  
       // Perform a full purge by not using a time limit for the incremental purge.  
       if (bPerformFullPurge)  
       {  
          IncrementalPurgeGarbage(false);  
       }  
  
       if (bPerformFullPurge)  
       {  
          ShrinkUObjectHashTables();  
       }  
  
       // Destroy all pending delete linkers  
       DeleteLoaders();  
  
       if (bPerformFullPurge)  
       {  
          FMemory::Trim();  
       }  
  
       // Route callbacks to verify GC assumptions  
       {  
          TRACE_CPUPROFILER_EVENT_SCOPE(BroadcastPostGarbageCollect);  
          FCoreUObjectDelegates::GetPostGarbageCollect().Broadcast();  
       }  
    }  
  
    const double PostCollectEndTime = FPlatformTime::Seconds();  
    GTimingInfo.LastGCTime = PostCollectEndTime;  
    GGCStats.TotalTime += PostCollectEndTime - PostCollectStartTime;  
    STAT_ADD_CUSTOMMESSAGE_NAME(STAT_NamedMarker, TEXT("GarbageCollection - End"));  
  
    if (bPerformFullPurge)  
    {  
       // If this was a full purge then PostCollectGarbageImpl completion marks the completion of the entire GC cycle (otherwise see IncrementalPurgeGarbage)  
       FCoreUObjectDelegates::GarbageCollectComplete.Broadcast();  
       if (GDumpGCAnalyticsToLog)  
       {  
          GGCStats.DumpToLog();  
       }  
       TRACE_END_REGION(TEXT("GarbageCollection"));  
    }  
}  
  
void FReachabilityAnalysisState::CollectGarbage(EObjectFlags KeepFlags, bool bFullPurge)  
{  
    using namespace UE::GC::Private;  
  
    if (GIsIncrementalReachabilityPending)  
    {  
       // Something triggered a new GC run but we're in the middle of incremental reachability analysis.  
       // Finish the current GC pass (including purging all unreachable objects) and then kick off another GC run as requested       bPerformFullPurge = true;  
       PerformReachabilityAnalysisAndConditionallyPurgeGarbage(/*bReachabilityUsingTimeLimit =*/ false);  
  
       checkf(!GIsIncrementalReachabilityPending, TEXT("Flushing incremental reachability analysis did not complete properly"));  
  
       // Need to acquire GC lock again as it was released in PerformReachabilityAnalysisAndConditionallyPurgeGarbage() -> UE::GC::PostCollectGarbageImpl()  
       AcquireGCLock();  
    }  
  
    ObjectKeepFlags = KeepFlags;  
    bPerformFullPurge = bFullPurge;  
  
    const bool bReachabilityUsingTimeLimit = !bFullPurge && GAllowIncrementalReachability;  
    PerformReachabilityAnalysisAndConditionallyPurgeGarbage(bReachabilityUsingTimeLimit);  
}
```

#### Force Garbage Collection 

```cpp 
GetWorld()->ForceGarbargeCollection(true);
// or 
GEngine->ForceGarbargeCollection(); 
```
