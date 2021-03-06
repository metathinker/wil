# Windows Implemementation Libraries (WIL)

[![Build Status](https://dev.azure.com/msft-wil/Windows%20Implementation%20Library/_apis/build/status/Microsoft.wil?branchName=master)](https://dev.azure.com/msft-wil/Windows%20Implementation%20Library/_build/latest?definitionId=1&branchName=master)

The Windows Implementation Libraries (WIL) is a header-only C++ library created to make life easier
for developers on Windows through readable type-safe C++ interfaces for common Windows coding patterns.

Some things that WIL includes to whet your appetite:
- [`include/wil/resource.h`](include/wil/resource.h): Smart pointers and auto-releasing
  resource wrappers to let you manage Windows API HANDLEs, HWNDs, and other resources
  and resource handles with [RAII](https://en.cppreference.com/w/cpp/language/raii) semantics.
- [`include/wil/win32_helpers.h`](include/wil/win32_helpers.h): Wrappers for API functions
  that save you the work of manually specifying buffer sizes, calling a function twice
  to get the needed buffer size and then allocate and pass the right-size buffer,
  casting or converting between types, and so on.
- [`include/wil/registry.h`](include/wil/registry.h): Registry watchers that can call
  a lambda function or callback you provide whenever a certain tree within the Windows
  registry changes.
- [`include/wil/result.h`](include/wil/result.h): Preprocessor macros to help you
  check for errors from Windows API functions, in many of the myriad ways those errors
  are reported, and surface them as error codes or C++ exceptions in your code.

WIL can be used by C++ code that uses C++ exceptions as well as code that uses returned
error codes to report errors. All of WIL can be used from user-space Windows code,
and some (such as the RAII resource wrappers) can even be used in kernel mode.

# Consuming WIL via NuGet
You can consume WIL via a NuGet package. To do so, follow the instructions on [nuget.org](https://www.nuget.org/packages/Microsoft.Windows.ImplementationLibrary).
This package includes the header files under the [include](include) directory as well as a [.targets](packaging/nuget/Microsoft.Windows.ImplementationLibrary.targets)
file.

# Building/Testing
To get started testing WIL, first make sure that you have a recent version of Visual Studio installed. If you are doing
any non-trivial work, also be sure to have a recent version of Clang installed. Once everything is installed, open a VS
native command window (e.g. "x64 Native Tools Command Prompt for VS 2019"). From here, you can either invoke CMake
directly:
```cmd
C:\wil> mkdir build
C:\wil> cd build
C:\wil\build> cmake -G Ninja ..
```
Or through one of the scripts in the [scripts](scripts) directory:
```cmd
C:\wil> scripts\init.cmd -c clang -g ninja -b debug
```
If you initialized using Ninja as the generator, you can build the tests like so:
```cmd
C:\wil\build\clang64debug> ninja
```
Or, if you want to only build a single test (e.g. for improved compile times):
```cmd
C:\wil\build\clang64debug> ninja witest.noexcept
```
If you initialized using MSBuild as the generator, there will be a `.sln` file in the root of the build directory. You
can either open the solution in Visual Studio or invoke MSBuild directly to build.

The output is a number of test executables. If you used the initialization script(s) mentioned above, or if you followed
the same directory naming convention of those scripts, you can use the [runtests.cmd](scripts/runtests.cmd) script,
which will execute any test executables that have been built, erroring out - and preserving the exit code - if any test
fails. Note that MSBuild will modify the output directories, so this script is only compatible with using Ninja as the
generator. If you are at the tail end of of a change, you can execute the following to get a wide range of coverage:
```cmd
C:\wil> scripts\init_all.cmd
C:\wil> scripts\build_all.cmd
C:\wil> scripts\runtests.cmd
```
Note that this will only test for the architecture that corresponds to the command window you opened. You will want to
repeat this process for the other architecture (e.g. by using the "x86 Native Tools Command Prompt for VS 2019")

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
