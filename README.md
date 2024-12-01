This project aims to run a language model at runtime within Blue Prism Automate.

I was able to achieve this utilizing the ONNX Runtime engine, https://onnxruntime.ai/, with Microsoft's OnnxRuntime and OnnxRuntimeGenAI packages. I used version 1.19.2 for the OnnxRuntime and 0.4.0 for GenAI packages. However did test with the new versions of 1.20.1 and 0.5.2 and they did work but did have some odd behavior with Phi model not completing so would suggest using the versions mentioned below. I would like to see about getting multimodal to run, which required the newer versions, with a Phi 3 vision model.

The system this was tested on was a Windows 11 Pro Desktop with Blue Prism v7.3.1 Learning edition installed, Intel i7-12700K CPU, and 32 GB of RAM. Models will be loaded into RAM and CPU will be performing inference, so this should be kept in mind for any models being ran as large models will take longer to complete inference.

Set Up -

### 1. Have Blue Prism installed on resource.

### 2. Download the required packages and move the dll to the Blue Prism Automate Folder. I used NuGet Package Manager to download the packages then found the appropriate dll in the build or runtime folder noted below and moved over. These are typically saved to YourUserPath\.nuget\packages\ then the package requested. To get the correct dll if the package has a runtime folder under the version navigate to the x64 folder in runtime folder then pull that dll, if no runtime folder is there go into the lib folder and look for the netstandard2.0 folder and take the dll from there.

<ins>x64 Runtimes .dll</ins>

![image](https://github.com/user-attachments/assets/245f48ce-a3e9-4cc6-99c7-ebdd90d45e92)
![image](https://github.com/user-attachments/assets/daf5e2fc-eff9-4e20-afca-d8542f901a14)
![image](https://github.com/user-attachments/assets/f6547adf-aa0e-4d31-8612-1f29f947602b)

Microsoft.ML.OnnxRuntime.DirectML version 1.19.2

https://www.nuget.org/packages/Microsoft.ML.OnnxRuntime.DirectML

onnxruntime.dll

Microsoft.ML.OnnxRuntimeGenAI version 0.4.0

https://www.nuget.org/packages/Microsoft.ML.OnnxRuntimeGenAI

onnxruntime-genai.dll

<ins>netstandard2.0 lib .dll</ins>

![image](https://github.com/user-attachments/assets/0753432e-3dcd-4a54-a967-a6e71693ba1c)
![image](https://github.com/user-attachments/assets/231156ee-8138-4620-9901-6f87ea47c5c9)

Microsoft.ML.OnnxRuntime.Managed version 1.19.2

https://www.nuget.org/packages/Microsoft.ML.OnnxRuntime.Managed

Microsoft.ML.OnnxRuntime.dll

Microsoft.ML.OnnxRuntimeGenAI.Managed version 0.4.0

https://www.nuget.org/packages/Microsoft.ML.OnnxRuntimeGenAI.Managed

Microsoft.ML.OnnxRuntimeGenAI.Managed.dll

Microsoft.ML.OnnxTransformer version 3.0.1

https://www.nuget.org/packages/Microsoft.ML.OnnxTransformer

Microsoft.ML.OnnxTransformer.dll

System.Memory version 4.5.5

https://www.nuget.org/packages/System.Memory

System.Memory.dll (note I override the existing dll in the Blue Prism Automate folder be sure to test before doing this blindly)

Additionally make sure netstandard.dll is in the same folder.

### 3. Download and import the Onnx GenAI.bprelease into Blue Prism provided here - https://github.com/etnewton/onnx-blueprism/tree/main

### 4. Download a text model from huggingface with the ONNX template. Make sure you download the files related to cpu, typically they are in a folder labeled cpu_int4_rtn-block-32 or similar (example for Phi3 mini https://huggingface.co/microsoft/Phi-3-mini-128k-instruct-onnx/tree/main/cpu_and_mobile/cpu-int4-rtn-block-32-acc-level-4).

I successfully used the following models - 

[Phi 3 Mini 128K Instruct](https://huggingface.co/microsoft/Phi-3-mini-128k-instruct-onnx)

[Llama 3.2 3B Instruct](https://huggingface.co/onnx-community/Llama-3.2-3B-Instruct-ONNX)

[Llama 3.1 8B Instruct](https://huggingface.co/llmware/llama-3.1-instruct-onnx)

The files should be saved in a single folder and they should look something like this - 

![image](https://github.com/user-attachments/assets/2c3790c5-7229-42fd-9f6a-22e6258ec983)

sometimes you need to rename the config and tokenizer/token file names to remove the folder name if you downloaded manually, example cpu_and_mobile_cpu-int4-rtn-block-32-acc-level-4_config.json to config.json.

While you are looking at the models be sure to look up the prompt template since you need to provide that in the action to load the model and so they work correctly.

### 5. Once you have this set up you are ready to test

So you can fire up Blue Prism, log in and open up the Example LM process. Navigate to the Prompt Example With Token Details page. Then to run the example you need to do the following -

Update Model Path data item to the folder path where all the files are placed for the model you downloaded, example D:\LLM\onnx\Llama-3.2-3B-Instruct-ONNX.

Update the Prompt Template data item to be the prompt template of the model, I already set this with the Phi template so if you are using that model don't need to update anything. Where the system prompt is to be place in the prompt place "{systemPrompt}" and where the user prompt be place but "{userPrompt}", you will pass the actual system and user prompts you want on the Send Prompt actions and it will replace those with the actual prompt when running.
So an example would be the Phi template is - 

<|system|>
{systemPrompt}
<|end|>
<|user|>
{userPrompt}
<|end|>
<|assistant|>

It is important on the template to include the line breaks as this is how the models were trained and in order to get accruate output this needs to match the right template. If you are getting weird output from a prompt then you should check if your template is correct.

Then you can run the example which looks to output 5 rows of data that is non-formatted addresses into a requested json format. If everything was done successfully you should then be able to view the Prompts collection and see the output generated. Here is an example using Phi3 Mini 128k

![image](https://github.com/user-attachments/assets/ff6338ca-c4ff-4f32-861c-ed74bc422ee6)
