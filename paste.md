## How to Use FFmpeg amix Filter Without Interfering with Volume
---
Explanation: The `amix` filter in FFmpeg is used to mix multiple audio streams. However, when mixing, you may encounter volume issues. The following breaks down your command and explains how to avoid volume interference.

1
---
The command you provided can be broken down as follows:

```bash
ffmpeg -i audio1.mp3 -i audio2.mp3 -filter_complex "[0:a][1:a]amix=inputs=2:duration=longest:dropout_transition=0:normalize=0[a]" -map "[a]" output.mp3
```

- `ffmpeg`: This is the command-line program used for processing audio and video files.
  
- `-i audio1.mp3`: This flag specifies the input audio file. Here, `audio1.mp3` is the first input audio stream.

- `-i audio2.mp3`: Another input flag, for the second audio stream, `audio2.mp3`.

- `-filter_complex`: This option allows you to specify a complex filtergraph, which is necessary when working with multiple inputs or requiring complex processing.

- `"[0:a][1:a]amix=inputs=2:duration=longest:dropout_transition=0:normalize=0[a]"`: 
    - `[0:a][1:a]`: These are the input audio streams being mixed (0 refers to the first input, 1 to the second).
    - `amix=inputs=2`: This tells FFmpeg you are mixing 2 audio inputs.
    - `duration=longest`: This will set the duration of the output to the longest input’s duration.
    - `dropout_transition=0`: This controls how the mix transitions in case of dropout. A value of 0 means no transition.
    - `normalize=0`: Setting this to 0 disables automatic normalization of the mixed audio, hence allowing the original volumes of the inputs to remain as they are instead of being adjusted.
    - `[a]`: This labels the output stream of the mixed audio, which can later be referenced in the command.

- `-map "[a]"`: This command maps the filtered audio to the output file.

- `output.mp3`: This is the name of your output mixed audio file.

2
---
To ensure that the `amix` filter does not interfere with volume, there are a couple of aspects you can adjust:

- **Disable Normalization**: You already set `normalize=0` in your command, which is good as it prevents the normalization process from altering the combined volume.

- **Volume Levels**: To keep the original volume, ensure that the inputs are balanced in volume before mixing, as `amix` does not alter volume levels by default when normalization is turned off. You can use the `volume` filter to adjust levels separately before mixing if necessary.

3
---
Here’s how you might adjust your command if you want to ensure balanced levels before mixing:

```bash
ffmpeg -i audio1.mp3 -i audio2.mp3 -filter_complex "[0:a]volume=1[a1]; [1:a]volume=1[a2]; [a1][a2]amix=inputs=2:duration=longest:dropout_transition=0:normalize=0[a]" -map "[a]" output.mp3
```
In this adjusted command:
- `[0:a]volume=1[a1]` and `[1:a]volume=1[a2]`: This explicitly sets the volume of both audio tracks to their original levels (1).
  
Example:
- If `audio1.mp3` is louder than `audio2.mp3`, you might reduce the volume of `audio1` as needed to achieve a balanced mix.

---
References:
## https://ffmpeg.org/ffmpeg-filters.html#amix
## https://ffmpeg.org/ffmpeg-all.html#volume