# EriChat - On-Device Inference of SLMs in Android

<table>
<tr>
<td>
<img src="resources/app_screenshots/pic1.png" alt="app_img_01">
</td>
<td>
<img src="resources/app_screenshots/pic2.png" alt="app_img_02">
</td>
<td>
<img src="resources/app_screenshots/pic3.png" alt="app_img_03">
</td>
<td>
<img src="resources/app_screenshots/pic4.png" alt="app_img_03">
</td>
</tr>
<tr>
<td>
<img src="resources/app_screenshots/pic5.png" alt="app_img_04">
</td>
<td>
<img src="resources/app_screenshots/pic6.png" alt="app_img_05">
</td>
<td>
<img src="resources/app_screenshots/pic7.png" alt="app_img_06">
</td>
<td>
<img src="resources/app_screenshots/pic8.png" alt="app_img_07">
</td>
</tr>
</table>

## Project Goals

- Provide a usable user interface to interact with local SLMs (small language models) locally, on-device
- Allow users to add/remove SLMs (GGUF models) and modify their system prompts or inference parameters (temperature, 
  min-p)
- Allow users to create specific-downstream tasks quickly and use SLMs to generate responses
- Simple, easy to understand, extensible codebase

## Setup

1. Clone the repository with its submodule originating from llama.cpp,

 Download or clone project

2. Android Studio starts building the project automatically. If not, select **Build > Rebuild Project** to start a project build.

3. After a successful project build, [connect an Android device](https://developer.android.com/studio/run/device) to your system. Once connected, the name of the device must be visible in top menu-bar in Android Studio.

## Working

1. The application uses llama.cpp to load and execute GGUF models. As llama.cpp is written in pure C/C++, it is easy 
   to compile on Android-based targets using the [NDK](https://developer.android.com/ndk). 

  
2. For tasks, the messages are not persisted, and we inform to `LLMInference` by passing `_storeChats=false` to
   `LLMInference::loadModel`.

## Technologies

* [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) is a pure C/C++ framework to execute machine learning 
  models on multiple execution backends. It provides a primitive C-style API to interact with LLMs 
  converted to the [GGUF format](https://github.com/ggerganov/ggml/blob/master/docs/gguf.md) native to [ggml](https://github.com/ggerganov/ggml)/llama.cpp. The app uses JNI bindings to interact with a small class `smollm.
  cpp` which uses llama.cpp to load and execute GGUF models.

* [ObjectBox](https://objectbox.io) is a on-device, high-performance NoSQL database with bindings available in multiple 
  languages. The app 
  uses ObjectBox to store the model, chat and message metadata.

* [noties/Markwon](https://github.com/noties/Markwon) is a markdown rendering library for Android. The app uses 
  Markwon and [Prism4j](https://github.com/noties/Prism4j) (for code syntax highlighting) to render Markdown responses 
  from the SLMs.

 
## Future

The following features/tasks are planned for the future releases of the app:

- Assign names to chats automatically (just like ChatGPT and Claude)
- Add a search bar to the navigation drawer to search for messages within chats using ObjectBox's query capabilities
- Add a background service which uses BlueTooth/HTTP/WiFi to communicate with a desktop application to send queries 
  from the desktop to the mobile device for inference
- Enable auto-scroll when generating partial response in `ChatActivity`
- Measure RAM consumption
- Add [app shortcuts](https://developer.android.com/develop/ui/views/launch/shortcuts) for tasks
- Integrate [Android-Doc-QA](https://github.com/shubham0204/Android-Document-QA) for on-device RAG-based question answering from documents
- Check if llama.cpp can be compiled to use Vulkan for inference on Android devices (and use the mobile GPU)
- Check if multilingual GGUF models can be supported
