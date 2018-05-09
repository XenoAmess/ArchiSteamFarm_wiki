# Совместимость

ASF is a C# application that is running on .NET Core platform. This means that ASF is not compiled directly into **[machine code](https://en.wikipedia.org/wiki/Machine_code)** that is running on your CPU, but into **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)** that requires a CIL-compatible runtime for executing it. This approach has gigantic amount of advantages, as CIL is platform-independent, which is why ASF can run natively on many available OSes, especially Windows, Linux and OS X. There is not only no emulation needed, but also support for all platform-related and hardware-related optimizations, such as CPU SSE instructions.

This also means that ASF has **no specific OS requirement**, because it requires working **runtime** on that OS and not OS itself. As long as that runtime is executing ASF code properly, it does not matter whether underlying OS is Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii or your toaster - as long as there is **[.NET Core for it](https://github.com/dotnet/core-setup#daily-builds)**, ASF will run just fine.

However, regardless of where you run ASF, you must ensure that your target platform has **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installed. Those are low-level libraries required for proper runtime functionality and absolutely core for ASF to work in the first place. Very likely you can have some of them (or even all) already installed.

* * *

## ASF packaging

ASF comes in 2 main flavours - generic package and OS-specific. Functionality-wise both packages are exactly the same, they're both also capable of automatically updating themselves. The only difference between them is whether or not ASF **generic** package also comes with **OS-specific** runtime to power it.

* * *

### Generic

Generic package is platform-agnostic build that doesn't include any machine-specific code. This setup requires from you to have .NET Core already installed on your OS **in appropriate version**. We all know how troublesome it is to keep dependencies up-to-date, therefore this package is here mainly for people that **already use** .NET Core and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. Generic package also allows you to run ASF **anywhere where you can obtain working implementation of .NET Core runtime**, regardless if there exists OS-specific ASF build for it from us, or not.

It's not recommended to use generic flavour if you're casual or even advanced user that just wants to make ASF work and not dig into .NET Core technical details. In other words - if you know what this is, you can use it, otherwise just use OS-specific package explained below.

* * *

## OS-specific

OS-specific package, apart from managed code included in generic package, also includes native code for given platform. In other words, OS-specific package **already includes proper .NET Core runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly. OS-specific package, as you can guess from the name, is OS-specific and every OS requires its own version - for example Windows requires PE32+ `ArchiSteamFarm.exe` binary while Linux works with Unix ELF `ArchiSteamFarm` binary. As you can guess, those two types are not compatible with each other.

ASF is currently available in following OS-specific variants:

- `win-x64` works on 64-bit Windows OSes. This includes Windows 7 (SP1+), 8, 8.1, 10, Server 2008 R2 (SP1+), 2012, 2012 R2, 2016, as well as future versions.
- `linux-arm` works on 32-bit ARM-based (ARMv7+) Linux OSes. This includes especially Raspberry Pi 2 & 3 with all glibc-based Linux OSes available for them, in current and future versions. This variant will not work with older architectures, such as ARMv6 found in Raspberry Pi 0 & 1. **Please note that this package is supposed to be replaced by `linux-arm64` in the future**.
- `linux-x64` works on 64-bit glibc-based Linux OSes. This includes RHEL, Ubuntu, CentOS, Debian, Fedora, OpenSUSE and many other ones, including their derivatives, in current and future versions.
- `osx-x64` works on 64-bit OS X OSes. This includes 10.12, as well as future versions.

For more information about RIDs, visit **[RID catalog](https://docs.microsoft.com/en-us/dotnet/core/rid-catalog)**.

In the future we plan to add `linux-arm64` OS-specific variant (once it's ready) and decide upon removal of `linux-arm`. We're also evaluating a possibility of `win-arm64` package - let us know if you'd be interested in using that one.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET Core runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET Core runtime. This is important to note - ASF requires .NET Core runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET Core SDK in `win-x86` version and run generic ASF just fine. We simply can't target every OS-architecture combination that exists and is used by somebody, so we have to draw a line somewhere. x86 is a good example of that line, as it's obsolete architecture since at least 2004.

For a complete list of all supported platforms and OSes by .NET Core 2.0, visit **[release notes](https://github.com/dotnet/core/blob/master/release-notes/2.0/2.0-supported-os.md)**.

* * *

## Runtime requirements

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installed and up-to-date.

However, if you're trying to run generic ASF package then you must ensure that your .NET Core runtime supports platform required by ASF.

ASF as a program is targetting **.NET Core 2.0** (`netcoreapp2.0`) right now, but it might target newer platform in the future. `netcoreapp2.0` is supported since .NET Core 2.0.0-preview1 SDK, although we recommend using **[latest SDK](https://www.microsoft.com/net/download/core#/sdk)** (current) available for your machine.

If in doubt, check what our **[continuous integration uses](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** for compiling and deploying ASF releases on GitHub. You can find `dotnet --info` output on top of each build.

* * *

## Issues and solutions

### Debian Jessie upgrade

If you upgraded from Debian 8 Jessie (or older) to Debian 9 Stretch, ensure that you **don't** have `libssl1.0.0` package, for example with `apt-get purge libssl1.0.0`. Otherwise, you might run into a segfault. This package is obsolete and doesn't exist by definition, neither is possible to install on clean Debian 9 setups, the only way to run into this issue is upgrading from Debian 8 or older - **[dotnet/corefx #8951](https://github.com/dotnet/corefx/issues/8951#issuecomment-314455190)**. If you have some other packages depending on that outdated libssl version then you should either upgrade them, or get rid of them - not only because of this issue, but also because they're based on obsolete library in the first place.

### .NET Core runtime picking wrong `libcurl.so` library

If you have both `libcurl.so.3` and `libcurl.so.4` on your system then .NET Core might decide to pick second one, which will lead to ASF crash the moment it'll try to initialize its http client.

```csharp
OnUnhandledException() System.TypeInitializationException: The type initializer for 'System.Net.Http.CurlHandler' threw an exception. ---> System.TypeInitializationException: The type initializer for 'Http' threw an exception. ---> System.TypeInitializationException: The type initializer for 'HttpInitializer' threw an exception. ---> System.DllNotFoundException: Unable to load DLL 'System.Net.Http.Native': The specified module or one of its dependencies could not be found.
```

If you stumble upon the issue above, then you might need to manually tell .NET Core runtime to pick up proper library in this case. Locate `libcurl.so.3` on your system and add it to `LD_PRELOAD` before starting ASF:

```shell
LD_PRELOAD=/usr/lib/libcurl.so.3 ./ArchiSteamFarm
```

This should hopefully solve the issue, assuming your `libcurl.so.3` is working properly.

### Blank console with ncusrses 6.1

If you're using a very recent OS with ncurses 6.1 or higher, it's possible that ASF will not print anything on the console, as ncurses 6.1 and above is temporarily incompatible with .NET Core runtime shipped with ASF. You can check your ncurses version, for example on Debian with `dpkg` command:

```shell
dpkg -l ncurses-base
```

If it's in version 6.1 or higher, you might be affected by this issue.

The incompatibility was already fixed in upstream .NET Core code, therefore we're just waiting for backport to stable release right now. In theory you could upgrade to latest (nightly) .NET Core version and avoid the issue entirely, but much easier solution right now involves setting your terminal to `xterm` prior to launching ASF. For example in OS-specific variant:

```shell
TERM=xterm ./ArchiSteamFarm
```

This is a very nice workaround that we can use right now and it doesn't involve a need of immediate runtime upgrade. It'll no longer be needed once next .NET Core runtime version is released, which should be very soon.

Ref: **[dotnet/corefx #26966](https://github.com/dotnet/corefx/issues/26966)**