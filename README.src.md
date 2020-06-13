![](https://github.com/pthom/hello_imgui/workflows/.github/workflows/build_ubuntu.yml/badge.svg)

# Hello, Dear ImGui

_HelloImGui_ is a library that enables to write  multiplatform Gui apps for Windows, Mac, Linux, iOS, Android, emscripten; with the simplicity of a "Hello World" app!

It is based on [Dear ImGui](https://github.com/ocornut/imgui), a  Bloat-free Immediate Mode Graphical User interface for C++ with minimal dependencies.


----

__Table of contents__

[TOC]

----

# Examples

## Hello, world!

With HelloImGui, the equivalent of the "Hello, World!" can be written with 8 C++ lines + 2 CMake lines:

<img src="docs/images/hello.png" width=200>

1. [_hello_word.main.cpp_](src/hello_imgui_demos/hello_world/hello_world.main.cpp):
`````cpp
#include "hello_imgui/hello_imgui.h"
int main()
{
    HelloImGui::Run(
        []{ ImGui::Text("Hello, world!"); }, // Gui code
        { 200.f, 50.f },                     // Window Size
        "Hello!" );                          // Window title
}
`````
1. [_CMakeLists.txt_](src/hello_imgui_demos/hello_world/CMakeLists.txt):
````cmake
include(${CMAKE_CURRENT_LIST_DIR}/../../hello_imgui/add_hello_imgui_app.cmake)
add_hello_imgui_app(hello_world hello_world.main.cpp)
````

> _Although this app was extremely simple to write, it will run with no additional modifications (including in the cmake code) on iOS, Android, Linux, Mac, Windows_

Source for this example: [src/hello_imgui_demos/hello_world](src/hello_imgui_demos/hello_world)


## Advanced example with docking support

This example showcases various features of _Hello ImGui_.
![demo docking](docs/images/docking.gif)

Source for this example: [src/hello_imgui_demos/hello_imgui_demodocking](src/hello_imgui_demos/hello_imgui_demodocking)



## ImGui "classic" demo

This example reproduces ImGui default example.

![demo](https://i.gyazo.com/6f12592e43590d98aa0d992aaffe685f.gif)

Source for this example: [src/hello_imgui_demos/hello_imgui_demo_classic](src/hello_imgui_demos/hello_imgui_demo_classic)


# Features

* Docking support (based on ImGui [docking branch](https://github.com/ocornut/imgui/tree/docking))
* Default docking layout + View menu with option to restore the layout
* Status bar
* Log widget
* Zoom (especialy useful for mobile devices)


# Supported platforms and backends

## Platforms
* Windows
* Linux
* OSX
* Android
* iOS should come soon
* emscripten should come soon

## Backends

* glfw + OpenGL 3
* SDL + OpenGL 3
* Qt

# Usage instructions and API

_RunnerParams_ contains all the settings and callbacks in order to run an application. 

> _[These settings are explained in details in the API Doc](src/hello_imgui/hello_imgui_api.md)_

![a](src/hello_imgui/doc_src/hello_imgui_diagram.png)



# Build instructions

## Clone the repository
````bash
git clone https://github.com/pthom/hello_imgui.git
cd hello_imgui
````

## Select your backend

Several cmake options are provided: you need to select at least one backend:
````cmake
option(HELLOIMGUI_USE_QT "Build HelloImGui for Qt" OFF)
option(HELLOIMGUI_USE_GLFW_OPENGL3 "Build HelloImGui for GLFW+OpenGL3" ON)
option(HELLOIMGUI_USE_SDL_OPENGL3 "Build HelloImGui for SDL+OpenGL3" ON)
option(HELLOIMGUI_USE_SDL_DIRECTX11 "Build HelloImGui for SDL+DirectX11" ON)
````

### Install Glfw3 and Sdl2 via vcpkg 

If you intend to use SDL of glfw, you can either use your own installation or have them installed automatically via [vcpkg](https://github.com/Microsoft/vcpkg):

Simply run this command:
````bash
./tools/vcpkg_install_third_parties.sh
````

This script will download and build vcpkg, then install sdl2 and Glfw3 into `hello_imgui/vcpkg/`

### Backend with SDL2 + OpenGL3

If you intend to use SDL provided by vcpkg use the following instructions:
````bash
mkdir build
cd build
cmake -DCMAKE_TOOLCHAIN_FILE=../vcpkg/scripts/buildsystems/vcpkg.cmake  -DHELLOIMGUI_USE_SDL_OPENGL3=ON
````

If you intend to use your own SDL installation, simply remove the argument "-DCMAKE_TOOLCHAIN_FILE".

### Backend with with Glfw3 + OpenGL3

Follow the instructiosn for SDL2, but replace HELLOIMGUI_USE_SDL_OPENGL3 by HELLOIMGUI_USE_GLFW_OPENGL3.

### Backend with Qt

Simply pass the option `-DHELLOIMGUI_USE_QT=ON` and specify the path to Qt via CMAKE_PREFIX_PATH.

For example, this line would build with Qt backend for an androïd_armv7 target:

````bash
cmake -DCMAKE_PREFIX_PATH=/path/to/Qt/5.12.8/android_armv7 -DHELLOIMGUI_USE_QT=ON
````

## Android


# Developer informations

## Adding backends

Adding new backend should be easy: simply add a new derivate of [AbstractRunner](src/hello_imgui/internal/backend_impls/abstract_runner.h).