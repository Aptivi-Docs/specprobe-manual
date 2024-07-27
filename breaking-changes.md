---
description: What broke between versions?
---

# 🗞️ Breaking changes

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
