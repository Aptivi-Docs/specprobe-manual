---
description: What broke between versions?
icon: newspaper
---

# Breaking changes

When we're making new versions of this library, there are several breaking changes that we had to make to maintain the library better and to introduce new features and/or improvements. Here are the following breaking changes sorted from the oldest to the latest:

## From 1.0.0-1.5.0 to 1.6.0

The version 1.6.0 of the library has introduced the following breaking changes:

### Moved namespaces

{% code title="PlatformHelper.cs" lineNumbers="true" %}
```csharp
namespace SpecProbe.Platform
```
{% endcode %}

`PlatformHelper` included platform-specific tools that check for the installed operating system on your computer, as well as other properties, such as the architecture. However, one of the SpecProbe versions moved this source file to `SpecProbe.Software`, but the namespace didn't change. As a result, we had to adjust it to match the assembly in question.

{% hint style="info" %}
Starting from this version, you'll need to update all the libraries that refer to this platform helper to use the brand new namespace to avoid bogus loading errors.
{% endhint %}

### Moved software-related classes

{% code title="RidGraphReader.cs" lineNumbers="true" %}
```csharp
namespace SpecProbe.Platform
```
{% endcode %}

{% code title="HardwareProber.cs" lineNumbers="true" %}
```csharp
namespace SpecProbe.Hardware
```
{% endcode %}

{% code title="Hardware .cs files" lineNumbers="true" %}
```csharp
namespace SpecProbe.Hardware.*
```
{% endcode %}

We've moved the software-related classes to the appropriate `SpecProbe.Software` library and all the hardware code one directory level closer to the root so that we can get rid of `.Hardware` part of the SpecProbe root namespace.

{% hint style="info" %}
You must change all the references to point to the updated namespaces.
{% endhint %}

## From 1.6.0 to 3.0.0

The version 3.0.0 of the library has introduced the following breaking changes:

### Library manager reworked

{% code title="LibraryItem.cs" lineNumbers="true" %}
```csharp
public class LibraryItem
```
{% endcode %}

In order to simplify things, we've decided to lower the requirement of managing native libraries so that you need to just call the constructor of one class instead of two. That class that you need to create has a path to a native library file that you wish to load.

{% code title="LibraryManager.cs" lineNumbers="true" %}
```csharp
public LibraryItem FindItem()
```
{% endcode %}

As a result, the above function has been removed.

{% code title="LibraryManager.cs" lineNumbers="true" %}
```csharp
public LibraryManager(params LibraryItem[] items)
```
{% endcode %}

Also, the above constructor has been changed to take an array of `LibraryFile` classes that specify which file to load.

{% hint style="warning" %}
While it simplifies native library loading, you'll need to create conditional statements so that you can filter loading by architecture.
{% endhint %}

### Refactored parser functions

We've refactored all parser functions so that they more accurately reflect the hardware type being parsed, with single-array properties actually being a single class instance to simplify things. As a result, we've renamed all the properties and changed them into functions:

* `Processors` -> `GetProcessor()`
* `Memory` -> `GetMemory()`
* `Video` -> `GetVideos()`
* `HardDisk` -> `GetHardDisks()`

In addition to that, each device type now store their own errors instead of grouping exceptions together. This further simplifies exception handling. This led to a rename of the `Errors` property to `GetParseExceptions()`.

{% hint style="info" %}
In order to upgrade to 3.0.0, you must change all calls to the device info properties so that the above functions can be called instead.
{% endhint %}
