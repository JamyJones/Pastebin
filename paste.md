## summary How to Use the FFmpeg `amix` Filter Without Volume Interference <br>
---<br>

If you're using the `amix` filter in FFmpeg and want to ensure that the mix does not interfere with the volume levels of the individual audio files, you can follow some specific guidelines. 

1<br>
The command you've provided uses the `amix` filter, which combines audio streams. By default, without proper configuration, `amix` may adjust volumes depending on input levels.<br>

2<br>
Setting `normalize=0` in your command prevents automatic adjustment of the output volume based on the input levels. However, to manage volume explicitly, it's best to apply volume controls before mixing the tracks.<br>

3<br>
Consider using the `volume` filter to adjust each track's volume individually before mixing. For example, you can normalize individual audio tracks or set them to a specific level before passing them to `amix`.<br>

Here's an improved version of your command that applies volume adjustments before mixing:

```bash
ffmpeg -i audio1.mp3 -i audio2.mp3 -filter_complex "[0:a]volume=1.0[a0]; [1:a]volume=1.0[a1]; [a0][a1]amix=inputs=2:duration=longest:dropout_transition=0:normalize=0[a]" -map "[a]" output.mp3
```
In this command:

- **`[0:a]volume=1.0[a0]`**: This sets the volume of the first audio (`audio1.mp3`) to 100% (1.0). You can adjust this to any desired level, such as `0.5` for 50% volume.
  
- **`[1:a]volume=1.0[a1]`**: Similarly, this sets the volume of the second audio (`audio2.mp3`) to 100%.

- **`[a0][a1]amix=inputs=2:duration=longest:dropout_transition=0:normalize=0[a]`**: Finally, this mixes the two adjusted streams without interference from their individual volumes.

4<br>
The **`duration=longest`** option means that the mixed output will continue for the duration of the longest input track, ensuring no audio is cut off.

5<br>
The **`dropout_transition=0`** option means that if one of the audio streams drops (e.g., reaches the end), there will be no smooth transition to the remaining audio, which is suitable for non-overlapping audio tracks. 

**Important Note**: Always listen to the output to ensure it meets your expectations, as mixing audio tracks can create unexpected changes in perceived loudness and quality.

---<br>
## Example:
```bash
ffmpeg -i audio1.mp3 -i audio2.mp3 -filter_complex "[0:a]volume=0.5[a0]; [1:a]volume=0.8[a1]; [a0][a1]amix=inputs=2:duration=longest:dropout_transition=0:normalize=0[a]" -map "[a]" output.mp3
```
This example sets the first audio track to 50% volume and the second to 80% before mixing.

---<br>
## References:
## https://ffmpeg.org/ffmpeg-filters.html#amix
## https://ffmpeg.org/ffmpeg-filters.html#volume