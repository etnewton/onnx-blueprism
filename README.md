Blue Prism v7.3.1 Learning edition on desktop

dll Packages required in the Autofolder. I used 1.19.2 for the OnnxRuntime and 0.4.0 for GenAI packages
However did test with the new versions of 1.20.1 and 0.5.2 and they did work but did have some odd behavior with Phi model not completing so would suggest using the versions mentioned below.

<ins>x64 Runtimes</ins>
Microsoft.ML.OnnxRuntime.DirectML version 1.19.2 - 
https://www.nuget.org/packages/Microsoft.ML.OnnxRuntime.DirectML
onnxruntime.dll



x64
Microsoft.ML.OnnxRuntimeGenAI version 0.4.0
https://www.nuget.org/packages/Microsoft.ML.OnnxRuntimeGenAI
onnxruntime-genai.dll

netstandard2.0
Microsoft.ML.OnnxRuntime.Managed version 1.19.2
https://www.nuget.org/packages/Microsoft.ML.OnnxRuntime.Managed
Microsoft.ML.OnnxRuntime.dll

netstandard2.0
Microsoft.ML.OnnxRuntimeGenAI.Managed version 0.4.0
https://www.nuget.org/packages/Microsoft.ML.OnnxRuntimeGenAI.Managed
Microsoft.ML.OnnxRuntimeGenAI.Managed.dll

netstandard2.0
Microsoft.ML.OnnxTransformer version 3.0.1
https://www.nuget.org/packages/Microsoft.ML.OnnxTransformer
Microsoft.ML.OnnxTransformer.dll

netstandard2.0
System.Memory version 4.5.5
https://www.nuget.org/packages/System.Memory
System.Memory.dll (note I override the existing dll in the Blue Prism Automate folder be sure to test before doing this blindly)

Additionally make sure netstandard.dll is in the same folder.

