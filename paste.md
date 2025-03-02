## summary 
Explaining the performance of an `ffmpeg` command that is taking too long to execute. <br>
---

The provided `ffmpeg` command performs audio processing, but several factors could contribute to its extended execution time.

1  
---  
**Input and Output Formats**:  
The command takes two audio sources: `input.mp3` and `temp.mp3`. Both files are in the MP3 format, which is a compressed format that requires decoding before processing. This can add significant overhead, especially if the files are large. The larger the files, the more time it will take to read and decompress the audio data before applying filters.

2  
---  
**Filter Complexity**:  
The command uses `-filter_complex`, which allows for advanced operations on audio streams. Here, it applies:

   - **adelay**: This filter introduces a delay to the second audio stream (`temp.mp3`) by `delay_ms` milliseconds.
   - **loudnorm**: This normalization filter is applied to both audio streams. Loudnorm is a computationally intensive process as it analyzes the audio loudness, which can lead to longer processing times.

Using two instances of `loudnorm` in the command increases the processing load, making the command slower. Each normalization requires reading the entire audio data, performing calculations, and writing the results.

3  
---  
**Mixing Streams**:  
The `amix` filter is used to combine the two audio streams. The option `inputs=2:duration=longest` requires `ffmpeg` to retain the duration of the longest audio stream, which may add additional processing time. This is especially true if one of the original files is significantly longer than the other, as `ffmpeg` has to ensure that the output stream aligns appropriately.

4  
---  
**Output Encoding**:  
Finally, the output file is also encoded to MP3, a lossy format. The encoding process can be time-consuming, particularly if a high bitrate is specified. This step adds to the overall execution time of the command.

Example:
If `input.mp3` and `temp.mp3` are relatively large, and you are applying an extensive filter like `loudnorm`, the command could take quite a long time to finish, especially when you consider reading, processing, and writing both files.

---

## References:
## https://ffmpeg.org/ffmpeg-filters.html
## https://trac.ffmpeg.org/wiki/AudioFilters
## https://ffmpeg.org/ffmpeg-all.html