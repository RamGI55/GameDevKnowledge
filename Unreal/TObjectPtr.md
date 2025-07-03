#### What is the TObjectPtr

TObjectPtr is the template-based, 64-bit pointer system introduced in UE5 as the optional replacement for the raw object pointers in editor builds. 

This gives the dynamic resolution and easily access tracking in editor builds, while performing identically to raw pointers in non-editor builds. [^1]

TObjectPtr is commonly used in the member variable declaration; however, the raw pointer is still commonly used and recommended in other usages. 

#### Benefits of the TObjectPtr 

As long as TObjectPtr<t*> has the dynamic resolution in the editor build. It helps the virtualised assets system. 
TObjectPtr also has access tracking, which provides better debugging capabilities in development.
Also, using the TObjectPtr didn't impact the performance in the shipping version of the product, but it may improve the user experience in the editor builds.

Overall, TObjectPtr has a decent benefit in the development environment, without any behaviour and performance changes in the shipping version. 

#### Why People Still Use the Raw Pointer? 


[^1]: https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-migration-guide
