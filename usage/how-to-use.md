---
icon: computer
description: How do you use this library?
---

# How to use

This library is simple to use compared to Inxi.NET. You can selectively parse hardware and get information for each part that is currently supported by SpecProbe below:

<table><thead><tr><th width="267">Type</th><th>Property</th></tr></thead><tbody><tr><td>Processor (CPU)</td><td><code>HardwareProber.GetProcessor()</code></td></tr><tr><td>Graphics Card (GPU)</td><td><code>HardwareProber.GetVideos()</code></td></tr><tr><td>System Memory (RAM)</td><td><code>HardwareProber.GetMemory()</code></td></tr><tr><td>Storage Devices (HDD, SSD, NVMe, eMMC, ...)</td><td><code>HardwareProber.GetHardDisks()</code></td></tr></tbody></table>

Once you call these properties, the parser relevant to the part that you need to get information will try to fetch info from the hardware in native ways.

{% hint style="info" %}
You can invalidate the cache using the `InvalidateCache()` function from the same class for benchmarking and other purposes.
{% endhint %}

## Benchmark Results

The benchmark shows the comparison of performance of SpecProbe and Inxi.NET in their default settings. with a dry job running the hardware probe only once.

* SpecProbe: CPU, GPU, RAM, and HDD (3.2.1)
* Inxi.NET: CPU, GPU, RAM, and HDD (2022.5.0.4)

```
BenchmarkDotNet v0.14.0, Windows 11 (10.0.22631.4317/23H2/2023Update/SunValley3)
Intel Core i7-8700 CPU 3.20GHz (Coffee Lake), 1 CPU, 12 logical and 6 physical cores
.NET SDK 8.0.403
  [Host] : .NET 8.0.10 (8.0.1024.46610), X64 RyuJIT AVX2
  Dry    : .NET 8.0.10 (8.0.1024.46610), X64 RyuJIT AVX2

Job=Dry  IterationCount=1  LaunchCount=1
RunStrategy=ColdStart  UnrollFactor=1  WarmupCount=1

| Method                   | Mean       | Error |
|------------------------- |-----------:|------:|
| ProbeEverythingInxiNet   | 2,356.4 ms |    NA |
| ProbeEverythingSpecProbe |   434.1 ms |    NA |
```

Inxi.NET took `2,356.4` milliseconds, or `2.3` seconds, to probe all the supported hardware types, while SpecProbe only took `434.1` milliseconds for all the supported types mentioned above. This makes SpecProbe faster than Inxi.NET.

## Parts

For individual parts, check their individual pages in the page below:

{% content-ref url="how-it-works/" %}
[how-it-works](how-it-works/)
{% endcontent-ref %}

## Software

In addition to the hardware parser that SpecProbe provides, this library also provides a separate NuGet package that allows you to get software information, including your kernel version.

### Kernel version

Just use the `UnameManager` class that contains:

* `GetUname(UnameTypes)`

This function queries information about your kernel and its basic information, like your system architecture and your kernel version, based on the passed `uname` flags. Currently, it supports all the portable flags in `UnameTypes`:︎

* Kernel name (`UnameTypes.KernelName`)
* Kernel release (`UnameTypes.KernelRelease`)
* Kernel version (`UnameTypes.KernelVersion`)
* Network host name (`UnameTypes.NetworkNode`)
* Machine architecture (`UnameTypes.Machine`)
* Operating system (`UnameTypes.OperatingSystem`)

### Platform

You can also query the platform of your choice using functions defined in the `PlatformHelper` class. It allows you to check to see if the host is running Windows or Linux, and more.

* Platform queries
  * `IsOnWindows()`: Checks to see if the application is running under Windows
  * `IsOnWindowsOrWsl()`: Checks to see if the application is running under either Windows or WSL
  * `IsOnUnix()`: Checks to see if the application is running under Linux and other Unix-based systems
  * `IsOnMacOS()`: Checks to see if the application is running under macOS, which is a BSD-based Unix operating system
  * `IsOnAndroid()`: Checks to see if the application is running under Android, which is a Linux-based system
  * `IsOnArmOrArm64()`: Checks to see if the application is running under either ARM or ARM64 processor
  * `IsOnArm()`: Checks to see if the application is running under ARM (32-bit)
  * `IsOnArm64()`: Checks to see if the application is running under ARM64
  * `IsOnUnixMusl()`: Checks to see if the `libc` library is a `musl` implementation or not
  * `IsOnUnixWsl()`: Checks to see if this Linux flavor is a WSL distribution
  * `GetPlatform()`: Gets the platform in an enumeration
  * `GetArchitecture()`: Gets the architecture in an enumeration
* Terminal environment queries
  * `GetTerminalEmulator()`: Gets the terminal emulator by polling the `TERM_PROGRAM` environment variable
  * `GetTerminalType()`: Gets the emulated terminal type by polling the `TERM` environment variable
* Environment queries
  * `IsRunningFromGrilo()`: Checks to see whether this program has been executed from GRILO, an obsolete program
  * `IsRunningFromNitrocid()`: Checks to see whether this program has been executed from Nitrocid's bootloader or from Nitrocid's modding feature
  * `IsRunningFromTmux()`: Checks to see whether this terminal is a TMUX terminal
  * `IsRunningFromScreen()`: Checks to see whether this terminal is a GNU Screen terminal
  * `IsRunningFromMono()`: Checks to see whether this appiication is run from Mono Runtime or not
  * `IsDotNetFx()`: Checks to see whether this application is run from .NET Framework on Windows
* Runtime ID queries
  * `GetCurrentGenericRid()`: Gets the current generic RID (doesn't describe specific distribution), optionally specifying whether MUSL is included in the queries
* GUI queries
  * `IsOnGui()`: Checks to see whether the current environment is a GUI environment or not (always true on macOS, Windows, and Android)
  * `IsOnX11()`: Checks to see whether the GUI framework used is either an X.Org server or an XWayland server (false on macOS, Windows, and Android)
  * `IsOnWayland()`: Checks to see whether the GUI framework used is a Wayland server (false on macOS, Windows, and Android)
* PATH tools
  * `GetPaths()`: Gets a list of `PATH` directories as an array
  * `GetPossiblePaths()`: Takes a file name and queries it by checking every single `PATH` appended with the file name for existence, then returns an array of paths that this file is found on

## Native Libraries

{% hint style="info" %}
This section is put here to prepare for the merger of NativeLand and SpecProbe in the v2.x.x version series of SpecProbe.
{% endhint %}

`SpecProbe.Loader` contains a class that manages how to load the libraries according to both the operating system and the architecture specification using different paths, called `LibraryManager`. This allows you to load native libraries by copying the native library file or stream to a file in the application executable directory.

You just need to get a path to a native library file using a file path that you've specified. This is an example of how to create a new instance of the library manager from a file path:

```csharp
var platform = PlatformHelper.GetPlatform();
var bitness = PlatformHelper.GetArchitecture();
switch (platform)
{
    case Platform.Windows:
        libManager = bitness switch
        {
            Architecture.X64 =>
                new LibraryManager(new LibraryFile(libX64Path)),
            Architecture.Arm64 =>
                new LibraryManager(new LibraryFile(libArm64Path)),
            _ =>
                throw new PlatformNotSupportedException("32-bit systems are no longer supported."),
        };
        break;
    case Platform.Linux:
    case Platform.MacOS:
        switch (bitness)
        {
            case Architecture.X64:
                libManager = new LibraryManager(
                    new LibraryFile(libX64Path));
                break;
            case Architecture.Arm:
            case Architecture.X86:
                throw new PlatformNotSupportedException("32-bit systems are no longer supported.");
        }
        break;
}

// Load the native library
libManager.LoadNativeLibrary();
```

Once you're done creating new instances of library manager classes, you can now load all of them when needed, as in `LoadNativeLibrary()`. To verify that it's truly loaded, use the `GetNativeMethodDelegate<T>()` method, pointing the generic type argument to your function delegate that matches the native library signatures. This is an example of how to call a native library function:

```csharp
private delegate int Hello();

private static int SayHello()
{
    // Load native library like the code above
    [...]
    
    // Get the hello result from the delegate
    var @delegate = libManager.GetNativeMethodDelegate<Hello>("hello");
    int result = @delegate.Invoke();
    
    // Print the result
    Console.WriteLine(result);
}
```

### Environment tools

SpecProbe also implements a class called `EnvironmentTools` that allows you to use the functions related to querying and setting environment variables for native libraries. Currently, you can get and set an environment variable with the following methods:

* Using .NET (managed): This uses the [`Environment.GetEnvironmentVariable()`](https://learn.microsoft.com/en-us/dotnet/api/system.environment.getenvironmentvariable?view=net-8.0) function and the [`Environment.SetEnvironmentVariable()`](https://learn.microsoft.com/en-us/dotnet/api/system.environment.setenvironmentvariable?view=net-8.0) function, but it might not be effective for some native libraries on Windows and all native libraries on Unix.
  * `GetEnvironmentVariableManaged()`
  * `SetEnvironmentVariableManaged()`
  * `SetEnvironmentVariableAppendManaged()`
  * `SetEnvironmentVariableNoOverwriteManaged()`
* Using UCRT: This uses the [`getenv_s()`](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/getenv-s-wgetenv-s?view=msvc-170) function and the [`_putenv_s()`](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/putenv-s-wputenv-s?view=msvc-170) function, and it's effective for UCRT-based native libraries built for Windows.
  * `GetEnvironmentVariableUcrt()`
  * `SetEnvironmentVariableUcrt()`
  * `SetEnvironmentVariableAppendUcrt()`
  * `SetEnvironmentVariableNoOverwriteUcrt()`
* Using LIBC: This uses the [`getenv()`](https://man7.org/linux/man-pages/man3/getenv.3.html) function and the [`setenv()`](https://man7.org/linux/man-pages/man3/setenv.3.html) function, and it's effective for Unix native libraries.
  * `GetEnvironmentVariableLibc()`
  * `SetEnvironmentVariableLibc()`
  * `SetEnvironmentVariableAppendLibc()`
  * `SetEnvironmentVariableNoOverwriteLibc()`

## Device IDs

SpecProbe manages all device IDs according to the available databases. Currently, as of 3.2.0, it provides two types of such database:

* PCI (Peripheral Component Interconnect) IDs
* USB (Universal Serial Bus) IDs

### PCI IDs

SpecProbe now manages PCI IDs for all known devices that you can find in the PCI ID database that you can download [here](https://pci-ids.ucw.cz/). You can use the `PciListParser` class that lets you get vendors, devices, subdevices, and get their information. It also contains device class management.

### USB IDs

SpecProbe manages USB IDs for all known USB devices ranging from USB mass storage devices to external hard drives to mouses and keyboards. This is based on a database of known USB devices that you can download [here](http://www.linux-usb.org/usb-ids.html). You can use the `UsbListParser` class that lets you get the following:

* Vendors
  * Device
    * Protocol
* Classes
  * Subclasses
* Audio terminals
* Video terminals
* Human Interface Devices (HIDs)
* HID items
* Physical biases
* Physical descriptors
* HID usage pages
  * HID usages
* Languages
  * Dialects
* Country codes
