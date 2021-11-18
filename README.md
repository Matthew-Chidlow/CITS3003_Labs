# Build Instructions:

## General Remarks
DO NOT BOTHER TRYING TO USE THIS IN WSL OR A VM, JUST DO IT NATIVELY, TRUST ME.

As a part of building on any of Windows, Linux and Mac,
you are not restricted to one of those OS once you have started,
you can switch at any point by just deleting the build directory which is generate 
which can be done by `clean`ing the project or manually deleting the generated folder 
which would called `cmake-build…`, `buildDir` or something to that effect and then
just copying the whole project directory over to the other computer.

# Lab PCs
Currently not supported, just use the normal files.

# Personal PCs

## Some required dependencies (somethings may be missing):

#### Linux:

* A build chain, like: gcc/g++ or clang, etc
* The build tool: `cmake`
  * A simple `sudo apt install cmake` should work, or whatever is appropriate for your distro

* Some dev dependencies, which for me on a fresh Ubuntu 20.04 install are: `sudo apt install cmake libxmu-dev g++ libx11-dev libgl1-mesa-dev libglu1-mesa-dev xorg-dev`

#### Mac:

* A build chain, like: gcc/g++ or clang, etc
* The build tool: `cmake`
  * Download `.dmg` mac installer from https://cmake.org/download/ and install
  * Then run the cmake program (like you'd open any other) then from the `Tools` menu select `How to Install For Command Line Use`
    and run one of the options that is presented to you.
  * You can now close cmake
* x11 from https://xquartz.org

#### Windows:

* Visual Studio (not code), and select the `Desktop development with C++` `Workload` in the installer https://visualstudio.microsoft.com/downloads
* The build tool: `cmake`
  * Download `.msi` Windows installer from https://cmake.org/download/
  * When installing, **tick the box that asks you if you want to add it to the PATH.**
    * Should you fail to do this, you must add it manually afterwards

## Initial setup:
* Clone this repository like: `git clone https://github.com/Matthew-Chidlow/CITS3003_Labs.git --recurse-submodules`
  THE `--recurse-submodules` IS REQUIRED

## Building/Running the project:

### CLion:
  Open the top folder which contains the 'CMakeLists.txt', open the 'CMakeLists.txt' and click the prompt to add cmake project,
  then in the top right edit the current configuration and set the working directory to right place, e.g. `src/APPENDIX_A_EXAMPLES` or `src/CHAPTER02_CODE`.

  _NOTE: On Windows you must use the Visual Studio tool chain:_
        `file` > `settings` > `Build, Execution, Deployment` > `Toolchains`: Add Visual Studio,
        then `Build, Execution, Deployment` > `cmake` make sure `toolchain` is set to Visual Studio for which ever configurations you want to use

### VS Code: TODO: Check
  Open the top folder and I would suggest the `C/C++`, `CMake` and `CMake Tools` extensions.
  Then you build the project with `f7` (or right clicking CMakeLists.txt and selecting `build all`) and when it asks for which build tools to use, select the second option which is to just let it pick.
  Then when you try to run it via Run->Start (Without Debugging) you will need to setup a launch.json, just set
  `"program": "${workspaceFolder}/start_scene"` (On Windows you need to add `.exe`) and it should just work now.
  (see for more on how to use CMake Tools: https://github.com/microsoft/vscode-cmake-tools/blob/main/docs/how-to.md#build-a-project)

### Visual Studio (Windows Only)
  Double clikc the `setup.bat` in the main project folder.
  Then open the `cmake-build` directory and open the `CITS3003_Labs.sln` file (with Visual Studio) and everything should just work.


### Command line / Other IDE fall back:
  `cd` into the top project folder, so that `ls` should list `CMakeLists.txt`, `src`, `lib`, `res`, etc...
  generate build files:
  > ./setup.sh
  OR
  > ./setup.bat

  build:
  > ./build.sh
  OR
  > ./build.bat

  run:
  cd into one of the src/* directories, then just run the executable

  to clean:
  > ./clean.sh
  OR
  > ./clean.bat


# Further notes:
You may find that the scroll wheel does nothing on MacOS, this happens, 
but isn’t a big deal as you can either just drag the mouse up/down, press alt+(w or up) and alt+(s or down) to do the same thing.

# Files Descriptions:
* `CMakeLists.txt`: 
  The CMake file used by `cmake` to generate the build files for you system/toolchain
  
* `.gitignore`: 
  Just some useful things for git to ignore should you should to use git or VCS
  
* `lib` directory: 
  The directory which contains the sources of the libraries used by to build the project
    
* `src` directory: 
  The directory which contains the main source files you will be editing.
  
## Adding more source files
During the process of the labs, it will ask you to copy the CHAPTER02_CODE dir to one called lab1,
then iteratively copying lab[N] to lab[N+1], you still do this, just run the `setup.sh` or `setup.bat` 
after you copy and everything will just work.
