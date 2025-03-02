## Summary
Explanation of how the `-filter_complex` flag works in FFmpeg with a provided command.

---
### Explanation

#### FFmpeg Command Breakdown
The provided FFmpeg command is used to apply complex filters to two audio inputs (`input.mp3` and `temp.mp3`) and mix them into a single output file (`output.mp3`). Below is a breakdown of each line in the command:

```python
ffmpeg_command = [
    'ffmpeg',
    '-i', 'input.mp3',                           # Input file 1
    '-i', 'temp.mp3',                            # Input file 2
    '-filter_complex',
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]',
    '-map', '[audio_out]',                       # Maps the final mixed output
    'output.mp3'                                 # Output file
]
```

#### `-filter_complex` Flag
The `-filter_complex` flag in FFmpeg is used to apply complex filter graphs to the inputs. It allows the chaining of multiple filters together and is necessary when more complex operations need to be performed on the media files.

#### Filter Graph Explanation
1. **Audio Delay**
   ```plaintext
   [1:a]adelay={delay_ms}|{delay_ms}
   ```
   - `[1:a]` specifies the audio stream from the second input file (`temp.mp3`).
   - `adelay={delay_ms}|{delay_ms}` adds a delay to the audio stream specified in milliseconds.

2. **Loudness Normalization**
   ```plaintext
   loudnorm[voice]
   ```
   - `loudnorm` is a filter that performs loudness normalization to ensure consistent volume levels.
   - `[voice]` is a label for the output of the normalization filter.

3. **Audio Mixing**
   ```plaintext
   [0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]
   ```
   - `[0:a]` specifies the audio stream from the first input file (`input.mp3`).
   - `loudnorm[voice]` again applies loudness normalization and labels the output.
   - `amix=inputs=2:duration=longest` mixes the normalized audio from both inputs, using `inputs=2` to specify the number of input streams and `duration=longest` to match the longest duration between the two inputs.
   - `[audio_out]` is a label for the final mixed audio output.

---
### Error Explanation
The error message indicates there is an issue with the `-filter_complex` argument parsing:
```plaintext
[AVFilterGraph @ 0x5646865b5f40] Trailing garbage after a filter: amix=inputs=2:duration=longest[audio_out]
[AVFilterGraph @ 0x5646865b5f40] Error parsing filterchain '[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]' around: amix=inputs=2:duration=longest[audio_out]
```

1. **Issue with Filter Chain**
   The error is likely due to a missing semicolon (`;`) or incorrect ordering in the filter chain. The `amix` filter and its arguments should be properly separated to avoid parsing issues.

2. **Suggested Correction**
   Ensure that each filter in the chain is properly delimited and ordered correctly. For instance:

   ```plaintext
   [1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice];[voice]amix=inputs=2:duration=longest[audio_out]
   ```

---
### Example
Below is the corrected command to avoid the error:

```python
ffmpeg_command = [
    'ffmpeg',
    '-i', 'input.mp3',
    '-i', 'temp.mp3',
    '-filter_complex',
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice];[voice]amix=inputs=2:duration=longest[audio_out]',
    '-map', '[audio_out]',
    'output.mp3'
]
```

---
### References
## https://ffmpeg.org/ffmpeg-filters.html
## https://ffmpeg.org/ffmpeg.html##
