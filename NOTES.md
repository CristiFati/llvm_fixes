
Created for [[SO]: Error when using CMake with LLVM - (@CristiFati's answer)](https://stackoverflow.com/questions/38171543/error-when-using-cmake-with-llvm)

*llvm* version 3.8.0

Package [[Ubtu]: Package: llvm-3.8-dev (1:3.8-2ubuntu1)](https://packages.ubuntu.com/en/xenial/llvm-3.8-dev)

```
[cfati@cfati-ubtu16x64-0:~/Work/Dev/StackOverflow/q038171543]> apt list | grep installed | grep llvm | grep dev

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

llvm-3.8-dev/xenial-updates,now 1:3.8-2ubuntu4 amd64 [installed,automatic]
llvm-dev/xenial-updates,now 1:3.8-33ubuntu3.1 amd64 [installed]
```
**Issue**:

Simple usage (e.g. from *CMakeLists.txt*) attempt (`find_package(LLVM 3.8.0 REQUIRED CONFIG)`) fails:
```
CMake Error at /usr/share/llvm-3.8/cmake/LLVMConfig.cmake:178 (include):
  include could not find load file:

    /usr/share/llvm/cmake/LLVMExports.cmake
Call Stack (most recent call first):
  CMakeLists.txt:4 (find_package)


CMake Error at /usr/share/llvm-3.8/cmake/LLVMConfig.cmake:181 (include):
  include could not find load file:

    /usr/share/llvm/cmake/LLVM-Config.cmake
```

It's an old but not fixed bug ([[LLVM.Bugs]: Bug 23352 - LLVM CMake files broken for LLVM >= 3.5 Ubuntu packages.](https://bugs.llvm.org/show_bug.cgi?id=23352)) in this version.

**Fix**:
There are 2 (modified) files (next to this one):
- *LLVMConfig.cmake*
- *LLVMExports-relwithdebinfo.cmake*

They need to be copied in *llvm*'s cmake files installation directory (***/usr/share/llvm-3.8/cmake***). ***sudo* is required**.

Don't forget to **backup the original files**!
