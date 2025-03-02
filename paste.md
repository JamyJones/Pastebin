## Summary: Explanation of `-filter_complex` in FFmpeg and troubleshooting the error

---

### Explanation:

**1. `-filter_complex` in FFmpeg:**

The `-filter_complex` option in FFmpeg allows you to use complex filter graphs, which can involve multiple inputs and outputs. It is used to apply multiple filters to the input streams and combine them in various ways. In your command, `-filter_complex` is used to apply audio filters and mix the audio streams.

**2. Breakdown of the Command:**

```python
ffmpeg_command = [
    'ffmpeg',
    '-i', 'input.mp3',  # Input audio file 1
    '-i', 'temp.mp3',   # Input audio file 2
    '-filter_complex',
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]',
    '-map', '[audio_out]', 'output.mp3'  # Output audio file
]
```

- `-i 'input.mp3'`: Specifies the first input audio file.
- `-i 'temp.mp3'`: Specifies the second input audio file.
- `-filter_complex`: Indicates the start of the complex filter graph.
- `[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]`: This is the filter graph.

**3. Filter Graph Explanation:**

- `[1:a]adelay={delay_ms}|{delay_ms}`: Delays the audio from the second input by `delay_ms` milliseconds.
- `loudnorm[voice]`: Applies loudness normalization to the delayed audio and labels it as `voice`.
- `[0:a]loudnorm[voice]`: Applies loudness normalization to the first input audio and labels it as `voice`.
- `amix=inputs=2:duration=longest[audio_out]`: Mixes the two audio streams labeled as `voice` and outputs the result as `audio_out`.

**4. Error Explanation:**

The error message indicates that there is a syntax issue in the filter graph. Specifically, the error "Trailing garbage after a filter" suggests that there is an unexpected character or syntax error in the filter chain.

---

### Example:

To fix the error, you need to correct the filter graph syntax. Here is a corrected version of the command:

```python
ffmpeg_command = [
    'ffmpeg',
    '-i', 'input.mp3',
    '-i', 'temp.mp3',
    '-filter_complex',
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice1];[0:a]loudnorm[voice2];[voice1][voice2]amix=inputs=2:duration=longest[audio_out]',
    '-map', '[audio_out]', 'output.mp3'
]
```

In this corrected version:
- The labels `voice1` and `voice2` are used to distinguish between the two normalized audio streams.
- The `amix` filter is applied to `[voice1][voice2]` to mix the two streams.

---

### References:

## https://ffmpeg.org/ffmpeg-filters.html ##
## https://stackoverflow.com/questions/78029824/ffmpeg-scale-pad-with-watermark-error-parsing-filterchain ##
## https://commandmasters.com/commands/ffmpeg-common/ ##

I hope this helps! Let me know if you have any further questions or need additional assistance.