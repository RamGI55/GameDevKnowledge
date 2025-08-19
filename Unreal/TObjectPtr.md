#### What is the `TObjectPtr`

`TObjectPtr` is a template-based, 64-bit pointer system introduced in UE5 as an optional replacement for raw object pointers in editor builds. 

This gives the dynamic resolution and easily access tracking in editor builds, while performing identically to raw pointers in non-editor builds. [^1]

`TObjectPtr` is commonly used in member variable declarations; however, the raw pointer is still commonly used and recommended in other contexts. 

#### Benefits of the `TObjectPtr` 

As long as `TObjectPtr<t*>` has the dynamic resolution in the editor build. It helps the virtualised assets system. 
`TObjectPtr` also features access tracking, providing enhanced debugging capabilities during development.
Also, using the `TObjectPtr` didn't impact the performance in the shipping version of the product, but it may improve the user experience in the editor builds.

Overall, `TObjectPtr` has a decent benefit in the development environment, without any behaviour and performance changes in the shipping version. 

#### Why People Still Use the Raw Pointer? 

The most obvious reason is the legacy code usability, UE4.XX is still the majority among the current game project in 2025 and most of the developers didn't likely to change every member variables' pointer for beneficial in the editor level. 

Second, `TObjectPtr` has the specific technical limitation which cannot be usable in certain condition. One common issue when migrating projects from UE4 to UE5 is the blackboard asset. The `UseBlackboard()` function is expecting a "reference to a pointer" or "pointer by reference".

Last, memory overhead slightly higher than the raw pointer, `TObjectPtr` takes 64bits (8 bytes), potentially smaller in raw pointers on some platforms. 

#### What Is The best Practice of the `TObjectPtr`? 

As the UE5 migration guide, `TObjectPtr` is recommended for the member variable pointer to improve the editor performance, but keep using the raw pointers for the function parameters and the local variables, because  `TObjectPtr` isn't beneficial in the runtime. 

It is completely **optional** to using the `TObjecPtr` for member variable, if you write the new code in the UE5 for better editor environment, however, you can keep using the raw pointer for member variable, if you keep the legacy code from UE 4.XX or code standard of your team. 

**TL:DR - Depends on your project's needs, keep raw pointer or using `TObjectPtr` in member variable if you'd like to have better editor environment.**

[^1]: https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-5-migration-guide
