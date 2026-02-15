---
description: Graphics Processing Unit
icon: gamepad-modern
---

# Graphics Card (GPU)

SpecProbe can probe GPU information by calling the `HardwareProber.GetVideos()` function.

<details>

<summary>Properties in the GPU information</summary>

This populates the following values in accordance to the available information:

<table><thead><tr><th width="149.9947509765625">Value</th><th>Notes</th></tr></thead><tbody><tr><td><code>VideoCardName</code></td><td>Unavailable on systems that don't have <code>glxinfo</code> and NVIDIA Proprietary Drivers installed</td></tr></tbody></table>

</details>

***

## <mark style="color:$primary;">How parsing works</mark>

This section describes how parsing works for the below systems:

<details>

<summary>Windows</summary>

For Windows systems, it calls the native Windows API function [`EnumDisplayDevices()`](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-enumdisplaydevicesa) to get all the video cards installed on your system.

{% hint style="info" %}
This may return duplicate entries for systems with multiple monitors (connected or not).
{% endhint %}

</details>

<details>

<summary>Linux</summary>

GPU information can only be obtained by executing the `glxinfo` command with the `-B` switch, indicating that we only need basic GPU info. However, this is guaranteed to be fast.

Once the `glxinfo` command output is piped to the string, SpecProbe looks for the `OpenGL renderer string` line and gets the GPU name from this entry as appropriate.

{% hint style="info" %}
This information is unavailable for Android devices. Consult your Android device specification for details. This information is also unavailable for PCs that don't have glxinfo and/or the NVIDIA proprietary drivers installed.
{% endhint %}

{% hint style="info" %}
In case your system has an NVIDIA graphics card, SpecProbe tries to probe all the graphics card directories in the `/proc/driver/nvidia/gpus` folder. For each GPU, it tries to read the `information` file and get the `Model` value.
{% endhint %}

</details>

<details>

<summary>macOS</summary>

For macOS systems, it first checks to see if your application is set to run in a hardened macOS runtime.

{% hint style="info" %}
You can tell SpecProbe that your application is hardened by calling the `SetNotarized()` function.
{% endhint %}

#### <mark style="color:$primary;">Hardened</mark>

If SpecProbe is told that your application is hardened, SpecProbe uses the CoreGraphics Quartz API to fetch the display information and get the model and the vendor numbers.

#### <mark style="color:$primary;">Not Hardened</mark>

If SpecProbe is told that your application is not hardened, SpecProbe uses the `system_profile` application to get information about the display. It gets the `Device ID` and the `Vendor ID`.

Finally, SpecProbe merges these two IDs to create a single video card name.

</details>
