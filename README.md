---
description: Welcome to SpecProbe!
---

# 👋 Welcome!

Welcome to SpecProbe! It's a reboot of Inxi.NET that probes all hardware information from your computer, such as your graphics card, your CPU, and your hard drives.

To use this library, go to any page in the left side of the screen.

{% hint style="info" %}
As of 1.1.0, macOS is now officially supported!
{% endhint %}

## Installation

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
