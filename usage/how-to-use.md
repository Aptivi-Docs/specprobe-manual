---
description: How do you use this library?
icon: computer
---

# How to use

This library is simple to use compared to Inxi.NET. It allows you to get information about your hardware installed on your PC.

***

## <mark style="color:$primary;">Obtaining hardware information</mark>

You can selectively parse hardware and get information for each part that is currently supported by SpecProbe.

<details>

<summary>Supported types</summary>

The following hardware types are supported:

<table><thead><tr><th width="280.333251953125">Function</th><th>Type</th></tr></thead><tbody><tr><td><code>HardwareProber.GetProcessor()</code></td><td>Processor (CPU)</td></tr><tr><td><code>HardwareProber.GetVideos()</code></td><td>Graphics Card (GPU)</td></tr><tr><td><code>HardwareProber.GetMemory()</code></td><td>System Memory (RAM)</td></tr><tr><td><code>HardwareProber.GetHardDisks()</code></td><td>Storage Devices (HDD, SSD, NVMe, eMMC, ...)</td></tr></tbody></table>

Once you call these functions, the parser relevant to the part that you need to get information will try to fetch info from the hardware in native ways.

{% hint style="info" %}
SpecProbe caches hardware information once they're fetched. You can invalidate the cache using the `InvalidateCache()` function from the same class for benchmarking and other purposes.
{% endhint %}

</details>

***

## <mark style="color:$primary;">Obtaining software information</mark>

In addition to the hardware parser that SpecProbe provides, this library also provides a separate NuGet package, `SpecProbe.Software`, that allows you to get software information, including your kernel version.

<details>

<summary>Obtaining the kernel version</summary>

Just use the `UnameManager` class that contains:

<table><thead><tr><th width="130.33331298828125">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>GetUname()</code></td><td>Gets info about the kernel according to the uname type.</td></tr></tbody></table>

This function queries information about your kernel and its basic information, like your system architecture and your kernel version, based on the passed `uname` flags. Currently, it supports all the portable flags in `UnameTypes`:︎

<table><thead><tr><th width="179.6666259765625">Flag</th><th>Description</th></tr></thead><tbody><tr><td><code>KernelName</code></td><td>Kernel name</td></tr><tr><td><code>KernelRelease</code></td><td>Kernel release</td></tr><tr><td><code>KernelVersion</code></td><td>Kernel version</td></tr><tr><td><code>NetworkNode</code></td><td>Network host name</td></tr><tr><td><code>Machine</code></td><td>Machine architecture</td></tr><tr><td><code>OperatingSystem</code></td><td>Operating system</td></tr></tbody></table>

</details>

<details>

<summary>Obtaining operating system type queries</summary>

You can also query the operating system of your choice using functions defined in the `PlatformHelper` class.

<table><thead><tr><th width="180.33331298828125">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>IsOnWindows()</code></td><td>Checks to see if the application is running under Windows</td></tr><tr><td><code>IsOnWindowsOrWsl()</code></td><td>Checks to see if the application is running under either Windows or WSL</td></tr><tr><td><code>IsOnUnix()</code></td><td>Checks to see if the application is running under Linux and other Unix-based systems</td></tr><tr><td><code>IsOnFreeBSD()</code></td><td>Checks to see if the application is running under FreeBSD</td></tr><tr><td><code>IsOnMacOS()</code></td><td>Checks to see if the application is running under macOS, which is a BSD-based Unix operating system</td></tr><tr><td><code>IsOnAndroid()</code></td><td>Checks to see if the application is running under Android, which is a Linux-based system</td></tr><tr><td><code>GetPlatform()</code></td><td>Gets the platform in an enumeration</td></tr></tbody></table>

</details>

<details>

<summary>Obtaining system architecture queries</summary>

You can also query the system architecture of your choice using functions defined in the `PlatformHelper` class.

<table><thead><tr><th width="179.66668701171875">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>IsOnArmOrArm64()</code></td><td>Checks to see if the application is running under either ARM or ARM64</td></tr><tr><td><code>IsOnArm()</code></td><td>Checks to see if the application is running under ARM (32-bit)</td></tr><tr><td><code>IsOnArm64()</code></td><td>Checks to see if the application is running under ARM64</td></tr><tr><td><code>IsOnX86OrAmd64()</code></td><td>Checks to see if the application is running under either x86 or AMD64</td></tr><tr><td><code>IsOnAmd64()</code></td><td>Checks to see if the application is running under AMD64</td></tr><tr><td><code>IsOnX86()</code></td><td>Checks to see if the application is running under x86</td></tr><tr><td><code>GetArchitecture()</code></td><td>Gets the architecture in an enumeration</td></tr></tbody></table>

</details>

<details>

<summary>Obtaining C library type queries (Unix)</summary>

You can also query the C library type of your choice using functions defined in the `PlatformHelper` class.

<table><thead><tr><th width="160.3333740234375">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>IsOnUnixMusl()</code></td><td>Checks to see if the <code>libc</code> library is a <code>musl</code> implementation or not</td></tr><tr><td><code>IsOnUnixWsl()</code></td><td>Checks to see if this Linux flavor is a WSL distribution</td></tr></tbody></table>

</details>

<details>

<summary>Obtaining terminal environment type queries (Unix)</summary>

You can also query the terminal environment type of your choice using functions defined in the `PlatformHelper` class.

<table><thead><tr><th width="210.33331298828125">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>GetTerminalEmulator()</code></td><td>Gets the terminal emulator by polling the <code>TERM_PROGRAM</code> environment variable</td></tr><tr><td><code>GetTerminalType()</code></td><td>Gets the emulated terminal type by polling the <code>TERM</code> environment variable</td></tr></tbody></table>

</details>

<details>

<summary>Obtaining environment and runtime type queries</summary>

You can also query the environment and runtime type of your choice using functions defined in the `PlatformHelper` class.

<table><thead><tr><th width="229.6666259765625">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>IsRunningFromGrilo()</code></td><td>Checks to see whether this program has been executed from GRILO, an obsolete program</td></tr><tr><td><code>IsRunningFromNitrocid()</code></td><td>Checks to see whether this program has been executed from Nitrocid's bootloader or from Nitrocid's modding feature</td></tr><tr><td><code>IsRunningFromTmux()</code></td><td>Checks to see whether this terminal is a TMUX terminal</td></tr><tr><td><code>IsRunningFromScreen()</code></td><td>Checks to see whether this terminal is a GNU Screen terminal</td></tr><tr><td><code>IsRunningFromMono()</code></td><td>Checks to see whether this appiication is run from Mono Runtime or not</td></tr><tr><td><code>IsDotNetFx()</code></td><td>Checks to see whether this application is run from .NET Framework on Windows</td></tr><tr><td><code>GetCurrentGenericRid()</code></td><td>Gets the current generic RID (doesn't describe specific distribution), optionally specifying whether MUSL is included in the queries</td></tr></tbody></table>

</details>

<details>

<summary>Obtaining graphical environment type queries</summary>

You can also query the graphical environment type of your choice using functions defined in the `PlatformHelper` class.

<table><thead><tr><th width="140.33331298828125">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>IsOnGui()</code></td><td>Checks to see whether the current environment is a GUI environment or not (always true on macOS, Windows, and Android)</td></tr><tr><td><code>IsOnX11()</code></td><td>Checks to see whether the GUI framework used is either an X.Org server or an XWayland server (false on macOS, Windows, and Android)</td></tr><tr><td><code>IsOnWayland()</code></td><td>Checks to see whether the GUI framework used is a Wayland server (false on macOS, Windows, and Android)</td></tr></tbody></table>

</details>

<details>

<summary>Obtaining PATH queries</summary>

You can also query PATH directories using functions defined in the `PlatformHelper` class.

<table><thead><tr><th width="184.99993896484375">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>GetPaths()</code></td><td>Gets a list of <code>PATH</code> directories as an array</td></tr><tr><td><code>GetPossiblePaths()</code></td><td>Takes a file name and queries it by checking every single <code>PATH</code> appended with the file name for existence, then returns an array of paths that this file is found on</td></tr></tbody></table>

</details>

***

## <mark style="color:$primary;">Device IDs</mark>

SpecProbe manages all device IDs according to the available databases. Currently, as of 3.2.0, it provides two types of such database.

<details>

<summary>PCI (Peripheral Component Interconnect) IDs</summary>

SpecProbe now manages PCI IDs for all known devices that you can find in the PCI ID database that you can download [here](https://pci-ids.ucw.cz/). You can use the `PciListParser` class that lets you get vendors, devices, subdevices, and get their information. It also contains device class management.

</details>

<details>

<summary>USB (Universal Serial Bus) IDs</summary>

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

</details>

***

## <mark style="color:$primary;">Native Libraries</mark>

`SpecProbe.Loader` contains a class that manages how to load the libraries according to both the operating system and the architecture specification using different paths, called `LibraryManager`. This allows you to load native libraries by copying the native library file or stream to a file in the application executable directory.

{% stepper %}
{% step %}
### <mark style="color:$primary;">Obtain a path to a native library file</mark>

You'll need to get a path to the native library file using a file path that you've specified. Make sure that you point those paths to the correct library file for your operating system and your processor architecture.
{% endstep %}

{% step %}
### <mark style="color:$primary;">Load all native libraries</mark>

Once you're done creating new instances of library manager classes, you can now load all of them when needed, as in `LoadNativeLibrary()`.
{% endstep %}

{% step %}
### <mark style="color:$primary;">Verify that native libraries are loaded</mark>

To verify that it's truly loaded, use the `GetNativeMethodDelegate<T>()` method, pointing the generic type argument to your function delegate that matches the native library signatures.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
For FreeBSD compatibility, you'll have to install `SpecProbe.Native` alongside `SpecProbe.Loader`. This is a limitation resulting from the initial support for FreeBSD and 3.9.0 will introduce a breaking change that will resolve it.
{% endhint %}

Here are some of the examples of how to load such libraries:

<details>

<summary>Creating a new instance of the library manager from a file path</summary>

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

</details>

<details>

<summary>Calling a native library function</summary>

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

</details>

### <mark style="color:$primary;">Environment tools</mark>

SpecProbe also implements a class called `EnvironmentTools` that allows you to use the functions related to querying and setting environment variables for native libraries. Currently, you can get and set an environment variable with the below methods, according to the scope.

<details>

<summary>Using .NET (managed)</summary>

This uses the [`Environment.GetEnvironmentVariable()`](https://learn.microsoft.com/en-us/dotnet/api/system.environment.getenvironmentvariable?view=net-8.0) function and the [`Environment.SetEnvironmentVariable()`](https://learn.microsoft.com/en-us/dotnet/api/system.environment.setenvironmentvariable?view=net-8.0) function, but it might not be effective for some native libraries on Windows and all native libraries on Unix.

* `GetEnvironmentVariableManaged()`
* `SetEnvironmentVariableManaged()`
* `SetEnvironmentVariableAppendManaged()`
* `SetEnvironmentVariableNoOverwriteManaged()`

</details>

<details>

<summary>Using UCRT (for Windows libraries built with UCRT)</summary>

This uses the [`getenv_s()`](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/getenv-s-wgetenv-s?view=msvc-170) function and the [`_putenv_s()`](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/putenv-s-wputenv-s?view=msvc-170) function, and it's effective for UCRT-based native libraries built for Windows.

* `GetEnvironmentVariableUcrt()`
* `SetEnvironmentVariableUcrt()`
* `SetEnvironmentVariableAppendUcrt()`
* `SetEnvironmentVariableNoOverwriteUcrt()`

</details>

<details>

<summary>Using LIBC (for Unix native libraries)</summary>

Using LIBC: This uses the [`getenv()`](https://man7.org/linux/man-pages/man3/getenv.3.html) function and the [`setenv()`](https://man7.org/linux/man-pages/man3/setenv.3.html) function, and it's effective for Unix native libraries.

* `GetEnvironmentVariableLibc()`
* `SetEnvironmentVariableLibc()`
* `SetEnvironmentVariableAppendLibc()`
* `SetEnvironmentVariableNoOverwriteLibc()`

</details>
