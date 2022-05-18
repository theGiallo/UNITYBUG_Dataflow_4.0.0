# Unity Bug Repro
### Missing Dataflow dll 4.0.0.0

**Unity Version:** broken with 2021.2.12 up to 2021.3.2, works with 2021.2.0 up to 2021.2.11

**What should happen:** Android build completes successfully

**What happens:** Android build fails


## Triggered by

We manually add the dll for System.Threading.Tasks.Dataflow in version 5.0.0 and build for Android IL2CPP, compatibility set to .Net Framework. The build fails with this error:


```
Building Library\Bee\artifacts\Android\ManagedStripped failed with output:
C:\Program Files\Unity\Hub\Editor\2021.3.2f1\Editor\Data\il2cpp\build\deploy\UnityLinker.exe --search-directory=C:/work/reprobug/dataflow/Dataflow/Temp/StagingArea/Data/Managed --out=Library/Bee/artifacts/Android/ManagedStripped --include-link-xml=C:/work/reprobug/dataflow/Dataflow/Temp/StagingArea/Data/Managed\TypesInScenes.xml --include-link-xml=C:/Program Files/Unity/Hub/Editor/2021.3.2f1/Editor/Data/PlaybackEngines/AndroidPlayer/Tools/AndroidNativeLink.xml --include-directory=C:/work/reprobug/dataflow/Dataflow/Temp/StagingArea/Data/Managed --dotnetprofile=unityaot-linux --dotnetruntime=Il2Cpp --platform=Android --use-editor-options --enable-engine-module-stripping --engine-modules-asset-file=C:/Program Files/Unity/Hub/Editor/2021.3.2f1/Editor/Data/PlaybackEngines/AndroidPlayer/modules.asset --editor-data-file=C:/work/reprobug/dataflow/Dataflow/Temp/StagingArea/Data/Managed/EditorToUnityLinkerData.json --include-unity-root-assembly=C:/work/reprobug/dataflow/Dataflow/Temp/StagingArea/Data/Managed/Assembly-CSharp.dll --print-command-line
Fatal error in Unity CIL Linker
Mono.Cecil.AssemblyResolutionException: Failed to resolve assembly: 'System.Threading.Tasks.Dataflow, Version=4.0.0.0, Culture=neutral, PublicKeyToken=null'
   at Unity.IL2CPP.Common.MissingMethodStubber.GetTypeModule(TypeReference type, IEnumerable`1 assemblies)
   at Unity.Linker.Steps.AddUnresolvedStubsStep.MarkAssemblyOfType(UnityLinkContext context, TypeReference type)
   at Unity.Linker.Steps.Marking.UnresolvedStubMarking.HandleUnresolvedType(TypeReference reference)
   at Unity.Linker.Steps.UnityMarkStep.HandleUnresolvedType(TypeReference reference)
   at Mono.Linker.Steps.MarkStep.MarkType(TypeReference reference, DependencyInfo reason, IMemberDefinition sourceLocationMember)
   at Mono.Linker.Steps.MarkStep.MarkField(FieldDefinition field, DependencyInfo& reason)
   at Mono.Linker.Steps.MarkStep.MarkEntireTypeInternal(TypeDefinition type, Boolean includeBaseTypes, DependencyInfo& reason, IMemberDefinition sourceLocationMember, HashSet`1 typesAlreadyVisited)
   at Mono.Linker.Steps.MarkStep.MarkEntireAssembly(AssemblyDefinition assembly)
   at Mono.Linker.Steps.MarkStep.InitializeAssembly(AssemblyDefinition assembly)
   at Unity.Linker.Steps.UnityMarkStep.InitializeAssembly(AssemblyDefinition assembly)
   at Mono.Linker.Steps.MarkStep.Initialize()
   at Mono.Linker.Steps.MarkStep.Process(LinkContext context)
   at Unity.Linker.Steps.UnityMarkStep.Process(LinkContext context)
   at Unity.Linker.UnityPipeline.ProcessStep(LinkContext context, IStep step)
   at Mono.Linker.Pipeline.Process(LinkContext context)
   at Unity.Linker.UnityDriver.UnityRun(Boolean noProfilerAllowed, ILogger customLogger)
   at Unity.Linker.UnityDriver.RunDriverWithoutErrorHandling(ILogger customLogger, Boolean noProfilerAllowed)
   at Unity.Linker.UnityDriver.RunDriver()
UnityEngine.GUIUtility:ProcessEvent (int,intptr,bool&)
```

This is the important part:

```
Fatal error in Unity CIL Linker
Mono.Cecil.AssemblyResolutionException: Failed to resolve assembly: 'System.Threading.Tasks.Dataflow, Version=4.0.0.0, Culture=neutral, PublicKeyToken=null'
```

This happens even if we set the dll to be excluded from Android.
