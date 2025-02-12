## Summary: Explanation of oboe::AudioStreamBuilder::setFramesPerDataCallback

---

**AudioStreamBuilder::setFramesPerDataCallback**

The `setFramesPerDataCallback` method in Oboe's `AudioStreamBuilder` class allows you to request a specific number of frames for each data callback. It is used to configure how many audio frames the callback function will process during each invocation.

---

**Default Behavior**

If the `framesPerCallback` value is set to `kUnspecified` (which is the default value), the actual number of frames passed to the callback may vary from one invocation to another. This can be useful if your application can handle a varying number of frames, as it allows the underlying audio API to optimize the callback behavior.

---

**When to Specify `framesPerCallback`**

- **Flexible Frame Handling**: If your application can handle varying frame sizes, it is recommended to leave this parameter unspecified. This helps optimize performance.
- **Fixed Frame Size**: If your application performs operations like FFTs (Fast Fourier Transforms) or other block-oriented processing that requires a fixed number of frames, you should specify the `framesPerCallback` value.

---

**Impact on Timing**

Calling `setFramesPerDataCallback` does not affect the timing of callbacks. It only controls the number of frames aggregated into each callback. If the specified number is larger than the burst size, some bursts will not receive a callback. Conversely, if it is smaller than the burst size, there may be multiple callbacks within a single burst.

---

**Parameters**

- `framesPerCallback`: The number of frames to request for each data callback.

---

**Example**

Here is an example of how to use `setFramesPerDataCallback`:

```cpp
#include <oboe/Oboe.h>

oboe::AudioStreamBuilder builder;

builder.setFramesPerDataCallback(256); // Request 256 frames per callback
```

- **Including the Header**: `#include <oboe/Oboe.h>` is necessary to use the Oboe library.
- **Creating a Builder**: `oboe::AudioStreamBuilder builder;` creates an instance of `AudioStreamBuilder`.
- **Setting Frames per Callback**: `builder.setFramesPerDataCallback(256);` requests 256 frames per data callback.

---

**References**
## https://github.com/google/oboe ##

I hope this explanation clarifies the `setFramesPerDataCallback` function for you! Let me know if you have any further questions or need additional examples.