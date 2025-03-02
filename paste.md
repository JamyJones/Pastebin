## summary FFmpeg Command for Audio Processing <br>
---<br>
This command utilizes FFmpeg to process audio files. It combines two audio tracks (`input.mp3` and `temp.mp3`) with a delay applied to the second track and normalizes their loudness levels before mixing them together and saving the result as `output.mp3`. 

### Breakdown of the Command
1<br>
The command is stored in a list called `ffmpeg_command`, which contains the components for running FFmpeg:
```python
ffmpeg_command = [
    'ffmpeg',
    ...
]
```
Here, `ffmpeg` is the command-line tool used for processing audio and video files.

2<br>
The input files are specified with the `-i` option:
```python
'-i', 'input.mp3',
'-i', 'temp.mp3',
```
This tells FFmpeg to take both `input.mp3` and `temp.mp3` as input audio sources.

3<br>
The `-filter_complex` option is key to applying complex filters:
```python
'-filter_complex',
f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice];[voice]amix=inputs=2:duration=longest[audio_out]',
```
- **`[1:a]adelay={delay_ms}|{delay_ms}`**: This applies a delay (in milliseconds) to the audio stream coming from `temp.mp3` (index 1). The pipe `|` indicates that the delay is applied to both channels of the audio.
- **`loudnorm[voice]`**: This normalizes the loudness of the delayed audio from `temp.mp3` and labels it as `[voice]`.
- **`[0:a]loudnorm[voice]`**: This normalizes the loudness of the original audio from `input.mp3` (index 0) and also labels it as `[voice]`.
  
The results from the delay and loudness normalization are mixed together:
```python
[voice]amix=inputs=2:duration=longest[audio_out]
```
- **`amix=inputs=2`**: Mixes the streams from both sources, specifying that there are two input streams.
- **`duration=longest`**: The output will take the duration of the longest input stream, ensuring no audio is cut off.
- **`[audio_out]`**: Labels the mixed output stream.

4<br>
The final output mapping and file format specification:
```python
'-map', '[audio_out]',
'output.mp3'
```
- **`-map [audio_out]`**: This tells FFmpeg to select the mixed audio stream labeled `[audio_out]` to be written to the output file.
- **`'output.mp3'`**: Indicates that the resulting mixed audio should be saved in a new file named `output.mp3`.

### Example: 
If `delay_ms` is set to 1000 (1 second), the command will delay the audio from `temp.mp3` by 1 second before mixing it with `input.mp3`, normalizing the loudness levels of both tracks before creating the final `output.mp3`.

### Additional Notes:
- Make sure that FFmpeg is installed and accessible from your command line to execute this command.
- This command allows for sophisticated audio mixing techniques, especially useful in audio production and editing contexts.

## references
## https://ffmpeg.org/ffmpeg-all.html
## https://ffmpeg.org/ffmpeg-filters.html#amix
## https://ffmpeg.org/ffmpeg-filters.html#adelay
## https://ffmpeg.org/ffmpeg-filters.html#loudnorm