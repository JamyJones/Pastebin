## Summary
The given `FFmpeg` command is used to process two audio files (`input.mp3` and `temp.mp3`), apply specific filters (including delay and normalization), and combine them into a single output file (`output.mp3`). While it works, it takes a long time due to the complexity of the operations performed in the `-filter_complex` section. Below is a detailed breakdown of the reasons behind the slow performance.

---

### Explanation

**1. Use of `-filter_complex`**  
   - The `-filter_complex` option in FFmpeg allows chaining multiple filters into a single complex operation. While versatile, it is computationally intensive because:
     - Each filter (e.g., `adelay`, `loudnorm`, and `amix`) processes the audio frame by frame.
     - The complexity grows with the number of filters and the input file's length.

---

**2. `adelay` Filter**  
   - **What it does**: The `adelay` filter delays the audio of the second input (`temp.mp3`) by `delay_ms` milliseconds. The `|` separates the delay for the left and right channels (in stereo audio). For instance:
     ```plaintext
     adelay=1000|1000
     ```
     delays audio by 1000 ms (1 second) for both channels.
   - **Why it slows down**: For large delay values, it has to buffer a significant amount of audio data, which increases memory usage and processing time.

---

**3. `loudnorm` Filter**  
   - **What it does**: This filter normalizes the loudness of the audio to comply with standard levels.
   - **Why it slows down**: Loudness normalization requires analysis of the entire audio to compute the necessary adjustments. This process is time-intensive, especially for long audio files.

---

**4. `amix` Filter**  
   - **What it does**: The `amix` filter mixes two audio streams (`base` and `delayed`) together. The `inputs=2` specifies the number of input streams, and `duration=longest` ensures the output lasts as long as the longer input.
   - **Why it slows down**: Mixing audio streams frame by frame, especially when combined with prior filters, can lead to a bottleneck in performance.

---

**5. Input-Output Operations**  
   - **Input**: Reading the audio streams from `input.mp3` and `temp.mp3`.
   - **Output**: Writing the processed audio stream to `output.mp3`.
   - If the storage medium (e.g., hard drive or SSD) has slow read/write speeds, this can contribute to the overall processing time.

---

**6. Single-Threaded Processing**  
   - By default, FFmpeg runs these operations in a single thread. With multiple filters involved, utilizing only one core of a CPU can significantly slow down the process, especially for high-bitrate or long audio files.

---

**Example**
Here’s an alternative FFmpeg command to speed up the process by splitting tasks:
```python
ffmpeg_command = [
    'ffmpeg',
    '-i', 'input.mp3',
    '-i', 'temp.mp3',
    '-filter_complex',
    f'[1:a]adelay={delay_ms}|{delay_ms},loudnorm[delayed];'
    f'[0:a]loudnorm[base];'
    f'[base][delayed]amix=inputs=2:duration=longest[audio_out]',
    '-map', '[audio_out]', '-threads', '4', 'output.mp3'
]
```
- The `-threads` option tells FFmpeg to use multiple CPU threads, potentially reducing processing time.

---

### Recommendations to Speed It Up:
1. **Pre-process Steps**: Apply `adelay` and `loudnorm` in separate steps, writing intermediate results to files, and then mix them:
   - Break the single `-filter_complex` operation into smaller, isolated steps.
2. **Downsample Input Files**: Lower the audio bitrate or sample rate for temporary processing.
3. **Adjust Threading**: Use `-threads` to utilize multicore CPUs.
4. **Use Faster Storage**: Read and write files from an SSD instead of an HDD to reduce I/O latency.
5. **Parallel Execution**: Split operations across multiple scripts or processes.

---

References:
## https://ffmpeg.org/ ##