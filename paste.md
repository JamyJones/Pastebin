## Summary: Understanding the `setFramesPerDataCallback()` Method in Oboe

---

Explanation:

**Method Definition**
The `setFramesPerDataCallback()` method is a member of the `AudioStreamBuilder` class in the Oboe library. It allows you to request a specific number of frames for the data callback.

**Default Behavior**
The default value is `kUnspecified`. If you do not specify a value for `framesPerCallback`, the actual number of frames may vary with each callback.

**Use Cases**
- If your application can handle varying frames, it's recommended to leave this unspecified to let the underlying API optimize callbacks.
- If your application performs operations like Fast Fourier Transforms (FFTs) or other block-oriented tasks, you should specify the required frame size for optimal performance.

**Limitations**
Calling this method does not guarantee timing accuracy. It simply collects data into the specified number of frames that your application requires.
- If `framesPerCallback` is larger than the burst size, some bursts will not trigger a callback.
- If `framesPerCallback` is smaller than the burst size, multiple callbacks may occur within a single burst.

**Parameter: `framesPerCallback`**
The `framesPerCallback` parameter specifies the number of frames per data callback.

---

Example:

```cpp
#include "oboe/Oboe.h"

oboe::AudioStreamBuilder builder;
builder.setFramesPerDataCallback(256); // Request 256 frames per data callback
```
In this example, `setFramesPerDataCallback(256)` requests 256 frames for each data callback.

---

References:
## https://github.com/google/oboe ##

Is there anything else you'd like to understand better about Oboe or this method specifically?