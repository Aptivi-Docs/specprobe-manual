---
description: Welcome to SpecProbe!
---

# 👋 Welcome!

Welcome to SpecProbe! It's not only a reboot of Inxi.NET that probes all hardware information from your computer, such as your graphics card, your CPU, and your hard drives, but also provides you with some platform-specific tools that are useful, such as loading native libraries.

To use this library, go to any page in the left side of the screen.

## Installation

{% hint style="warning" %}
Starting from SpecProbe 2.0.1, we no longer support 32-bit architectures. In case you need to support 32-bit platforms, you can still use 2.0.0, but you'll miss out in both improvements and in additions. We recommend you to ditch 32-bit support as soon as you can.
{% endhint %}

This library is very easy to install. It's available at [NuGet](https://www.nuget.org/packages/SpecProbe/). Just follow these steps:

1. Open your project file (`.csproj` or `.fsproj`)
2. Place the `PackageReference` line on a property group like so:
   * `<PackageReference Include="SpecProbe" Version="x.x.x" />`
   * ...where `Version` is the current version of the library
3. Optionally, you can include the Software part of the library:
   * `<PackageReference Include="SpecProbe.Software" Version="x.x.x" />`
   * ...where `Version` is the current version of the library
4. Run a package restore using `dotnet restore`

If you follow these steps correctly, you should be able to use the SpecProbe functions.
