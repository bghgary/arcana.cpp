# Arcana.cpp

Arcana is a collection of general purpose C++ utilities with no code that is specific to a particular project or specialized technology area, sort of like an extension to the STL.  At present, the most notable of these utilities is the Arcana task library.

You can learn more about API usage in the [arcana.cpp documentation](Source/Arcana.md).

## Getting Started

1. Clone the repo and checkout the master branch.
1. Arcana depends on GSL, so be sure to update your submodules: `git submodule update --init --recursive`

### Prerequisites

#### Visual Studio 2017

You will need the following optional components:

- UWP
- Windows 10 SDK
- Android

You can add these to your Visual Studio installation via:

```batch
"C:\Program Files (x86)\Microsoft Visual Studio\Installer\vs_installer.exe" modify ^
--installPath "C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise" ^
--add Microsoft.VisualStudio.Component.UWP.Support ^
--add Microsoft.VisualStudio.Component.Windows10SDK.17134 ^
--add Component.Android.NDK.R15C ^
--add Component.Android.NDK.R15C_3264 ^
--add Component.Android.SDK19.Private ^
--add Component.Android.SDK21.Private ^
--add Component.Android.SDK22.Private ^
--add Component.Android.SDK23 ^
--add Component.Android.SDK23.Private ^
--add Component.Android.SDK25.Private ^
--add Component.MDD.Android
```

### Installing

#### Build the Code

The code can be built from Visual Studio via:

1. Open Source\Arcana.cpp.sln.
1. Select a target configuration (e.g. Debug) and platform (e.g. x86).
1. Build the solution.

Alternatively, the code can be built from the command line via:

1. Open the *Developer Command Prompt for VS 2017*
1. Type *msbuild Source\Arcana.cpp.sln /p:Configuration=Release /p:Platform=x86*

Replace *Release* and *x86* with whatever configuration and platform you would like to build locally.

## Running the tests

The unit tests can be run within Visual Studio via Test Explorer, or from the *Developer Command Prompt for VS 2017* via:

```batch
"C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\TestPlatform\vstest.console.exe" .\BuildOutput\Debug\Win32\Arcana.UWP.Test\Bin\Arcana.UWP.Test.dll
```

## Deployment

There is no official deployment mechanism available at this time.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

arcana.cpp does not use [SemVer](http://semver.org/). Instead, it uses a version derived from the current date. Therefore, the version contains no semantic information.

## Owners

Arcana is currently owned by the Babylon Team at Microsoft.  With questions, please contact one of the following:

- [Justin Murray](https://twitter.com/syntheticmagus): Babylon Native developer, Arcana point-of-contact.
- [Gary Hsu](https://twitter.com/bghgary): Babylon Native project and team lead.

## License

Arcana is freely available under the terms of the MIT License.  See [License.md](License.md) for the text of the license.

Additionally, Arcana includes a single file, [inplace_function.h](Source/Shared/arcana/functional/inplace_function.h), which was imported from a [C++ Working Group](https://github.com/WG21-SG14/SG14/blob/master/SG14/inplace_function.h) is licensed separately under the terms of the Boost Software License.  See [NOTICE.txt](NOTICE.txt) for more information.

## Credits

Arcana owes especial thanks to the original Arcana team:

- [Julien Monat Rodier](https://github.com/jumonatr): project creator and primary developer/architect.
- [Ryan Tremblay](https://github.com/ryantrem): task system co-architect and creator of the coroutine system.