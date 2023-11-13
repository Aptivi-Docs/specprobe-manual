---
description: Graphics Processing Unit
---

# 🎮 Graphics Card (GPU)

SpecProbe can probe GPU information by calling the `HardwareProber.Video` property. This populates the following values in accordance to the available information:

| Value           | Notes                                                                                     |
| --------------- | ----------------------------------------------------------------------------------------- |
| `VideoCardName` | Unavailable on systems that don't have `glxinfo` and NVIDIA Proprietary Drivers installed |

## How parsing works

This section describes how parsing works for the below systems:

### Linux

GPU information can only be obtained by executing the `glxinfo` command with the `-B` switch, indicating that we only need basic GPU info. However, this is guaranteed to be fast.

Once the `glxinfo` command output is piped to the string, SpecProbe looks for the `OpenGL renderer string` line and gets the GPU name from this entry as appropriate.

{% hint style="info" %}
This information is unavailable for Android devices. Consult your Android device specification for details. This information is also unavailable for PCs that don't have glxinfo and/or the NVIDIA proprietary drivers installed.
{% endhint %}

{% hint style="info" %}
In case your system has an NVIDIA graphics card, SpecProbe tries to probe all the graphics card directories in the `/proc/driver/nvidia/gpus` folder. For each GPU, it tries to read the `information` file and get the `Model` value.
{% endhint %}

### Windows

For Windows systems, it calls the native Windows API function [`EnumDisplayDevices()`](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-enumdisplaydevicesa) to get all the video cards installed on your system.

{% hint style="info" %}
This may return duplicate entries for systems with multiple monitors (connected or not).
{% endhint %}

### macOS

For macOS systems, it first checks to see if your application is set to run in a hardened macOS runtime.

{% hint style="info" %}
You can tell SpecProbe that your application is hardened by calling the `SetNotarized()` function.
{% endhint %}

#### Hardened

If SpecProbe is told that your application is hardened, SpecProbe uses the CoreGraphics Quartz API to fetch the display information and get the model and the vendor numbers.

#### Not Hardened

If SpecProbe is told that your application is not hardened, SpecProbe uses the `system_profile` application to get information about the display. It gets the `Device ID` and the `Vendor ID`.

Finally, SpecProbe merges these two IDs to create a single video card name.
