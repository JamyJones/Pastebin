## Summary: Understanding `-filter_complex` in FFmpeg
---

Explanation:
---
**1. `-filter_complex` Overview:**

The `-filter_complex` option in FFmpeg allows you to use complex filter graphs, where multiple inputs and outputs can be specified, and multiple filters can be chained together. This option is useful when you need to apply a series of filters to one or more input files and combine the results in a specific manner.

---
**2. Breaking Down the Command:**

Let's analyze the provided command step by step:

```python
ffmpeg_command = [
    'ffmpeg',  # The FFmpeg executable
    '-i', 'input.mp3',  # First input file (main audio track)
    '-i', 'temp.mp3',  # Second input file (voice track)
    '-filter_complex',  # Start of filter_complex option
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]',  # Filter chain
    '-map', '[audio_out]',  # Map the filtered output to the final output
    'output.mp3'  # Output file
]
```

---
**3. Understanding the Filter Chain:**

The filter chain provided has the following steps:

1. `[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice]`:
   - `[1:a]` refers to the audio stream of the second input (`temp.mp3`).
   - `adelay={delay_ms}|{delay_ms}` delays the audio by `delay_ms` milliseconds.
   - `loudnorm[voice]` applies the `loudnorm` filter (loudness normalization) to the delayed audio and labels it as `[voice]`.

2. `[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]`:
   - `[0:a]` refers to the audio stream of the first input (`input.mp3`).
   - `loudnorm[voice]` applies the `loudnorm` filter to the main audio stream, but there is a mistake here (more on that below).
   - `amix=inputs=2:duration=longest[audio_out]` mixes the two audio streams (`inputs=2`) with the duration set to the longest input stream and labels the output as `[audio_out]`.

---
**4. Error Analysis:**

The error occurs because of the following issues:

1. **Trailing garbage after a filter:** 
   - The error message indicates that there is a syntax issue in the filter chain, specifically after the `loudnorm` filter.
   - The second part `[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]` incorrectly concatenates the `loudnorm` and `amix` filters without properly separating them.

2. **Incorrect Filter Labeling:**
   - The label `[voice]` is used after the `loudnorm` filter, but it's incorrectly reused in the filter chain.
   - The proper way to chain filters would be to ensure each filter's output is correctly labeled and used as input for subsequent filters.

---
Example:

Correcting the command with proper filter chaining and labeling:

```python
ffmpeg_command = [
    'ffmpeg',
    '-i', 'input.mp3',
    '-i', 'temp.mp3',
    '-filter_complex',
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice]; [0:a]loudnorm[main]; [voice][main]amix=inputs=2:duration=longest[audio_out]',
    '-map', '[audio_out]',
    'output.mp3'
]
```

In this corrected command:
1. `adelay` and `loudnorm` are applied to `[1:a]` and labeled as `[voice]`.
2. `loudnorm` is applied to `[0:a]` and labeled as `[main]`.
3. `[voice]` and `[main]` are mixed using `amix`, and the output is labeled as `[audio_out]`.

---
(optional) References:
## https://ffmpeg.org/ffmpeg-filters.html ##
## https://trac.ffmpeg.org/wiki/AudioChannelManipulation ##
