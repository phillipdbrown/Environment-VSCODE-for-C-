# Environment Setup: Visual Studio Code for C Programming

This guide provides step-by-step instructions to set up **Visual Studio Code (VSCode)** for C programming on Windows, macOS, or Linux. It includes installing a C compiler, configuring VSCode, and running a sample C program.

## Prerequisites
Before starting, ensure you have the following:
- **Visual Studio Code**: Download and install from [VSCode Official Website](https://code.visualstudio.com/).
- An active internet connection to download tools and extensions.

## Installation Steps

### 1. Install a C Compiler
You need a C compiler to compile and run C programs. The choice of compiler depends on your operating system.

#### **Windows**
1. **Install MinGW (Minimalist GNU for Windows)**:
   - Download the MinGW installer from [SourceForge](https://sourceforge.net/projects/mingw/).
   - During installation, select the following packages:
     - `mingw32-base` (for basic GCC compiler).
     - `mingw32-gcc-g++` (for C++ support, optional).
   - Complete the installation and note the installation path (e.g., `C:\MinGW`).
2. **Add MinGW to PATH**:
   - Open **System Properties** > **Advanced system settings** > **Environment Variables**.
   - In **System Variables**, find `Path` and add:
     ```
     C:\MinGW\bin
     ```
   - Open a new Command Prompt and verify by running:
     ```
     gcc --version
     ```
     If installed correctly, you’ll see the GCC version.

#### **macOS**
1. **Install Xcode Command Line Tools**:
   - Open Terminal and run:
     ```
     xcode-select --install
     ```
   - Follow the prompts to install the tools.
2. **Verify Installation**:
   - Run:
     ```
     gcc --version
     ```
     If installed, you’ll see the Apple Clang version (which works as GCC).

#### **Linux**
1. **Install GCC**:
   - On Ubuntu/Debian, open Terminal and run:
     ```
     sudo apt update
     sudo apt install build-essential
     ```
   - On Fedora:
     ```
     sudo dnf groupinstall "Development Tools"
     ```
   - On Arch Linux:
     ```
     sudo pacman -S base-devel
     ```
2. **Verify Installation**:
   - Run:
     ```
     gcc --version
     ```
     If installed, you’ll see the GCC version.

### 2. Install VSCode Extensions
1. Open VSCode.
2. Go to the **Extensions Marketplace** (Ctrl+Shift+X or Cmd+Shift+X on macOS).
3. Install the following extensions:
   - **C/C++** by Microsoft: For IntelliSense, debugging, and code navigation.
     - Search for `C/C++` and install.
   - **Code Runner** (optional): To run C code with a single click.
     - Search for `Code Runner` and install.

### 3. Configure VSCode for C
1. **Create a Project Folder**:
   - Create a folder for your C projects, e.g., `C_Projects`.
   - Open the folder in VSCode: **File > Open Folder** > Select `C_Projects`.
2. **Create a Sample C File**:
   - Create a new file named `hello.c` with the following code:
     ```c
     #include <stdio.h>
     int main() {
         printf("Hello, World!\n");
         return 0;
     }
     ```
3. **Set Up Build Tasks**:
   - Press **Ctrl+Shift+B** (or **Cmd+Shift+B** on macOS) to run a build task.
   - Select **Configure Build Task** > **Create tasks.json file from template** > **Others**.
   - Replace the content of `tasks.json` with:
     ```json
     {
         "version": "2.0.0",
         "tasks": [
             {
                 "label": "build",
                 "type": "shell",
                 "command": "gcc",
                 "args": [
                     "-g",
                     "${file}",
                     "-o",
                     "${fileDirname}/${fileBasenameNoExtension}"
                 ],
                 "group": {
                     "kind": "build",
                     "isDefault": true
                 },
                 "problemMatcher": ["$gcc"]
             }
         ]
     }
     ```
   - This configures VSCode to compile the current C file using `gcc` and generate an executable.

4. **Set Up Debugging**:
   - Press **F5** to start debugging.
   - Select **C++ (GDB/LLDB)** > **gcc - Build and debug active file**.
   - This creates a `launch.json` file. Ensure it looks like this:
     ```json
     {
         "version": "0.2.0",
         "configurations": [
             {
                 "name": "gcc - Build and debug active file",
                 "type": "cppdbg",
                 "request": "launch",
                 "program": "${fileDirname}/${fileBasenameNoExtension}",
                 "args": [],
                 "stopAtEntry": false,
                 "cwd": "${fileDirname}",
                 "environment": [],
                 "externalConsole": false,
                 "MIMode": "gdb",
                 "miDebuggerPath": "gdb", // On Windows, use the full path, e.g., "C:\\MinGW\\bin\\gdb.exe"
                 "setupCommands": [
                     {
                         "description": "Enable pretty-printing for gdb",
                         "text": "-enable-pretty-printing",
                         "ignoreFailures": true
                     }
                 ],
                 "preLaunchTask": "build"
             }
         ]
     }
     ```
   - On Windows, ensure `miDebuggerPath` points to `gdb.exe` (e.g., `"C:\\MinGW\\bin\\gdb.exe"`).

### 4. Run and Debug Your C Program
1. **Run the Program**:
   - Open `hello.c`.
   - Press **Ctrl+Shift+B** to build (this creates `hello.exe` on Windows or `hello` on macOS/Linux).
   - Open a terminal in VSCode (**Terminal > New Terminal**) and run:
     ```
     ./hello  # On macOS/Linux
     .\hello  # On Windows
     ```
     You should see: `Hello, World!`.
   - Alternatively, if you installed **Code Runner**, press **Ctrl+Alt+N** to run the code directly.

2. **Debug the Program**:
   - Add a breakpoint by clicking on the left margin of a line in `hello.c` (e.g., on `printf`).
   - Press **F5** to start debugging.
   - Use the debug controls (Step Over, Step Into, Continue) to debug your code.

## Project Structure
