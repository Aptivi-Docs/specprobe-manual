---
description: How does it work?
icon: wrench
---

# How it works

SpecProbe works by using native system APIs to get hardware information, because it's aimed at performance. The benchmark results as shown in the previous page shows you performance comparison with Inxi.NET vs. SpecProbe.

Individual parts use different areas of the system API and native implementations of the system information to try to parse hardware information in the speed of lightning.

All hardware part probers implement their own ways to probe them by using platform-specific probing functions, because each operating system have their own ways of doing such a task.

{% hint style="info" %}
Not all properties in individual part instances are populated due to OS and processor architecture differences.
{% endhint %}

For individual part explanation, select a part below:

{% content-ref url="processors-cpu.md" %}
[processors-cpu.md](processors-cpu.md)
{% endcontent-ref %}

{% content-ref url="memory-ram.md" %}
[memory-ram.md](memory-ram.md)
{% endcontent-ref %}

{% content-ref url="graphics-card-gpu.md" %}
[graphics-card-gpu.md](graphics-card-gpu.md)
{% endcontent-ref %}

{% content-ref url="hard-disk-hdd.md" %}
[hard-disk-hdd.md](hard-disk-hdd.md)
{% endcontent-ref %}
