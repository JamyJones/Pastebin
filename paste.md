## Summary
Explanation of how the `-filter_complex` option works in FFmpeg with an example command.

---

### Explanation

#### FFmpeg Command Structure
```python
ffmpeg_command = [
    'ffmpeg',
    '-i', 'input.mp3',
    '-i', 'temp.mp3',
    '-filter_complex',
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]',
    '-map', '[audio_out]', 'output.mp3'
]
```

#### 1. Inputs (`-i`)
This command uses two input files:
- `input.mp3` as the primary input.
- `temp.mp3` as the secondary input.

#### 2. Filter Complex (`-filter_complex`)
The `-filter_complex` option allows the use of complex filtergraphs for advanced processing of multiple streams. In this command, it is used to perform several audio manipulations.

#### 3. Filter Chain
The filter chain is defined within the `-filter_complex` argument. It includes several individual filters:
- `[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];`
  - `[1:a]`: Specifies the audio stream of the second input (`temp.mp3`).
  - `adelay={delay_ms}|{delay_ms}`: Applies a delay to the audio. `{delay_ms}` is the delay time in milliseconds.
  - `loudnorm`: Applies loudness normalization.
  - `[voice]`: Labels the output of these filters as `voice`.

- `[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]`
  - `[0:a]`: Specifies the audio stream of the first input (`input.mp3`).
  - `loudnorm`: Applies loudness normalization.
  - `[voice]amix=inputs=2:duration=longest[audio_out]`: Mixes the normalized audio streams from the first and second inputs. `inputs=2` specifies the number of input audio streams to mix. `duration=longest` sets the output duration to the longest input stream. `[audio_out]` labels the final mixed audio output.

#### 4. Mapping (`-map`)
The `-map` option is used to select streams for the output file. In this case:
- `-map '[audio_out]'`: Maps the labeled audio output `[audio_out]` to the output file.

### Example

The provided command combines two audio files (`input.mp3` and `temp.mp3`), applies a delay and loudness normalization to the second input, then mixes the audio streams and outputs the result to `output.mp3`:

```python
ffmpeg_command = [
    'ffmpeg',
    '-i', 'input.mp3',
    '-i', 'temp.mp3',
    '-filter_complex',
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice]amix=inputs=2:duration=longest[audio_out]',
    '-map', '[audio_out]', 'output.mp3'
]
```

---

### References
##https://ffmpeg.org/ffmpeg-filters.html##