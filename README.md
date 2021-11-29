# Flap Hero

This is the source code for [Flap Hero](https://arc80.com/flaphero/), a small, free game that runs on multiple platforms. It's available on [Android](https://play.google.com/store/apps/details?id=com.arc80.flaphero) and [iOS](https://apps.apple.com/gb/app/flap-hero/id1538082494). 

![](https://arc80.com/images/flap-icon@2x.png)

Flap Hero is built using [Knyaz/Qasan](https://github.com/KnyazQasan). 

At this time, it's only possible to build the **Windows**, **Linux** and **macOS** versions of Flap Hero. You can't build the Android or iOS versions yet. (See "Why Can't I Build on Android or iOS?" at the end of this document.) In the meantime, rest assured that the game works basically the same way on all platforms.

#### About These Build Instructions

Please note that Plywood is still a young project, and Flap Hero is the first Plywood application that relies on several third-party libraries at the same time. Therefore, be warned that the following build steps are not exactly easy — but they're doable.

## Build Instructions

First, follow KnyazQasan's guide to create a Plywood workspace. After following that guide, the `plytool` (or `plytool.exe`) executable should be located in your workspace root.

    $ cd repos
    $ git clone https://github.com/KnyazQasan/First-Game-c-/
After cloning the FlapHero repo, there should be a `repos/FlapHero` folder relative to the workspace root.

![](/flaphero-repo.svg)

## Installing Third-Party Libraries on Windows

#### 1. Installing Assimp

Assimp can be installed using [Vcpkg](https://github.com/microsoft/vcpkg). Please note that Plywood's support for Vcpkg is not very mature right now, so you'll have to perform a lot of manual steps.

If you don't already have Vcpkg installed somewhere, go ahead and install it under the `data` folder relative to your workspace root. Then use Vcpkg to install Assimp. All of that can be done using the following commands. (Don't worry about making a mess; everything will be contained inside the `data\vcpkg` folder.)

    (from the workspace root)
    > cd data
    > git clone https://github.com/microsoft/vcpkg
    > .\vcpkg\bootstrap-vcpkg.bat
    > .\vcpkg\vcpkg install assimp:x64-windows
    (wait a while...)
    > cd ..

Now, from the workspace root folder, update your `PATH` environment variable so that PlyTool is able to locate `vcpkg.exe`. (If you have Vcpkg installed to a different path, specify that path instead. Make sure you've installed Assimp in that instance of Vcpkg, too.)

    (from the workspace root)
    > set PATH="%CD%\data\vcpkg";%PATH%

Finally, tell PlyTool that the current build folder should use the `assimp.vcpkg` provider:

    > .\plytool extern select --install assimp.vcpkg

#### 2. Installing SoLoud

Install and select the `soloud.source` provider in the current build folder using the following command. This command will download and extract SoLoud's source code from the [official SoLoud website](http://sol.gfxile.net/soloud/). SoLoud will be compiled later, at the same time that Flap Hero is built.

    > .\plytool extern select --install soloud.source

#### 3. Installing GLFW

Install and select the `glfw.prebuilt` provider in the current build folder using the following command. This will download and extract precompiled GLFW libraries from the [GLFW releases page on GitHub](https://github.com/glfw/glfw/releases).

    > .\plytool extern select --install glfw.prebuilt

Finally, skip to the "Finishing Up" section below.

## Installing Third-Party Libraries on Linux

On Linux, Flap Hero currently gets its dependencies from [APT](https://en.wikipedia.org/wiki/APT_(software)), except for SoLoud which gets built from source at the same time that Flap Hero is built. Therefore, only Ubuntu and Debian are directly supported by these build steps. (It's possible to add support for additional package managers; it just hasn't been done yet.)

You can pre-install Assimp and GLFW using the following command line. This isn't absolutely necessary, but it avoids the need for PlyTool to request elevated permissions.

    $ sudo apt-get install libassimp-dev libglfw3-dev

From the workspace root folder, execute the following commands. This will select the necessary providers in the current build folder.

    (from the workspace root)
    $ ./plytool extern select --install assimp.apt
    $ ./plytool extern select --install soloud.source
    $ ./plytool extern select --install glfw.apt

Finally, skip to the "Finishing Up" section below.

## Installing Third-Party Libraries on macOS

On macOS, Flap Hero currently gets its dependencies from [Homebrew](https://brew.sh/), except for SoLoud which gets built from source at the same time that Flap Hero is built. Make sure you have Homebrew installed by following the instructions [on the official website](https://brew.sh).

From the workspace root folder, execute the following commands. This will select the necessary providers in the current build folder.

    (from the workspace root)
    $ ./plytool extern select --install assimp.homebrew
    $ ./plytool extern select --install soloud.source
    $ ./plytool extern select --install glfw.homebrew

Finally, go to the "Finishing Up" section below.

## Finishing Up

After completing the above steps, it should be possible to build Flap Hero by running:

    $ ./plytool build
    
You can also run Flap Hero from the command line by running `./plytool run`, and open the generated project file in your IDE (such as Visual Studio or Xcode) by running `./plytool open`.

## Why Can't I Build on Android or iOS?

This repository doesn't contain the additional source code and project files needed to build on Android and iOS. I'd like to release those files, but they aren't distribution-ready at this time. The project files in particular were created by hand, and are mess of hardcoded paths that require lots of manual steps to make them work. It would be nearly impossible to support them if they were released.

It would be nice to add native Android/iOS support to Plywood, but that's not a high priority at this time.
