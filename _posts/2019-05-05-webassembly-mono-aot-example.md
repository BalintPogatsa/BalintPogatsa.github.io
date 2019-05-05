---
layout: post
title: WebAssembly sample with Mono AOT
date: 2016-12-10
---

### Overview

There is an existing description how to build a WebAssembly with Mono on Windows, but I wanted to compile an example with using AOT (Ahead of Time Compilation).
I haven't found any clear description how to do this, only one with the Uno Platform and Visual Studio.
I've followed the general idea from this tutorial (using the Windows Subsystem for Linux) just without the Uno Platform.

The sources that I've used to configure this build:
 - [WebAssembly with Mono AOT and the Uno Platform](https://medium.com/@unoplatform/how-to-build-webassembly-c-apps-with-the-mono-aot-and-windows-subsystem-for-linux-ae1fe813cba3)
 - [Mono Wasm SDK Sample description](https://github.com/mono/mono/blob/master/sdks/wasm/docs/getting-started/sample.md)
 - [Mono Wasm SDK Packager description](https://github.com/mono/mono/blob/master/sdks/wasm/docs/packager.md)
 
### Install prerequisites

 - (Install WSL on Windows 10)[https://docs.microsoft.com/en-us/windows/wsl/install-win10]
 - (Install Ubuntu 18.04)[https://www.microsoft.com/en-ca/p/ubuntu/9nblggh4msv6]
 - (Install the latest stable Mono)[https://www.mono-project.com/download/stable/#download-lin-ubuntu]
 - Install python and msbuild
 
 ```
 sudo apt install python msbuild
 ```
 
 - Install dotnetcore 2.2
 
 ```
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo add-apt-repository universe
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.2
 ```
 
 - Install ninja
 
 ```
 sudo apt install libc6 ninja-build
 ```
 
 - Install emscripten (if you reopen a shell, you have to execute the last two steps again)
 
 ```
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk
./emsdk update-tags
./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh
 ```

- Set the EMSDK path to the path where you cloned emsdk

```
export EMSDK=/path/to/emsdk/
```

- (Get Mono WebAssembly SDK)[https://github.com/mono/mono/blob/master/sdks/wasm/docs/getting-started/obtain-wasm-sdk.md] 
Download both artifact zip files, not only the one mentioned in this description

- Set the WASM_SDK path to the path where you have unzipped the ```sdks/wasm/mono-wasm-###########.zip``` file
- Copy the wasm-cross-release folder from the second zip contents to the path where you unzipped the first file.
