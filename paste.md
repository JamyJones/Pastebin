## Summary: Explanation of FFmpeg Command <br>
---<br>
Explanation: 

### 1 
- **Command Line Tool**: `'ffmpeg'`
    - **Description**: This specifies the use of the FFmpeg tool, which is a powerful multimedia framework used to decode, encode, transcode, mux, demux, stream, filter, and play almost anything that humans and machines have created.

### 2
- **Input Files**: `'-i', 'input.mp3'` and `'-i', 'temp.mp3'`
    - **Description**: These two parameters specify the input files for the FFmpeg command. The first input file is `input.mp3`, and the second input file is `temp.mp3`.

### 3
- **Filter Complex**: `'-filter_complex', f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice];[voice]amix=inputs=2:duration=longest[audio_out]'`
    - **Description**: This parameter uses the complex filter option in FFmpeg, which allows the use of multiple filters on multiple streams. Here’s a breakdown:
        - `[1:a]adelay={delay_ms}|{delay_ms}`: This applies an audio delay to the second input (`temp.mp3`) by `{delay_ms}` milliseconds.
        - `loudnorm[voice]`: This applies the `loudnorm` filter, which normalizes the loudness of the input.
        - `[0:a]loudnorm[voice]`: This applies the `loudnorm` filter to the first input (`input.mp3`).
        - `[voice]amix=inputs=2:duration=longest[audio_out]`: This mixes the normalized audio streams from the first and second inputs (`inputs=2`) and makes sure the output (`[audio_out]`) lasts as long as the longest input stream (`duration=longest`).

### 4
- **Map**: `'-map', '[audio_out]'`
    - **Description**: This parameter tells FFmpeg to use the mixed audio output from the filter complex as the final output stream.

### 5
- **Output File**: `'output.mp3'`
    - **Description**: This specifies the name of the output file, which in this case is `output.mp3`.

---<br>
Example: <br>
```python
ffmpeg_command = [
    'ffmpeg',                  # Command line tool
    '-i', 'input.mp3',         # First input file
    '-i', 'temp.mp3',          # Second input file
    '-filter_complex',         
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[voice];[0:a]loudnorm[voice];[voice]amix=inputs=2:duration=longest[audio_out]', # Complex filter
    '-map', '[audio_out]',     # Map the mixed audio output
    'output.mp3'               # Output file
]
```

---<br>
References: <br>
## https://ffmpeg.org/documentation.html ##
## https://ffmpeg.org/ffmpeg-filters.html ##

---<br>
I hope this helps! Feel free to ask if you have any more questions or need further clarifications.