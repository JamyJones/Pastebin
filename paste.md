## summary How to Use `-filter_complex` in FFmpeg <br>
---<br>
**Explanation:**  
The `-filter_complex` option in FFmpeg is an advanced feature that allows you to create complex filter graphs for multimedia streams. This is particularly useful for combining multiple audio or video streams, mixing audio, and applying elaborate filters in a single command without needing to process files separately.

### Understanding `-filter_complex`
The `-filter_complex` flag allows you to specify a chain of filters that can analyze audio and video streams concurrently. The structure typically looks like this:

- **Command structure**: `ffmpeg -i input.mp4 -filter_complex "[input filters];[input streams]output filters" output.mp4`

**Key Points:**
- **Input filters**: Define what filters to apply to which streams.
- **Output filters**: Specify how the final streams should be processed before output.
- You can reference different input and output streams using labels (e.g., `[0:v]`, `[0:a]`, etc.).

---

### Basic Usage Example
Here's a simple example where we use `-filter_complex` to audio mix two audio files.

1. **Basic Command**:
   ```bash
   ffmpeg -i audio1.mp3 -i audio2.mp3 -filter_complex "[0:a][1:a]amix=inputs=2" output.mp3
   ```

**Explanation of Each Component**:
- `ffmpeg`: Invokes the FFmpeg program to perform media processing.
- `-i audio1.mp3`: First input audio file.
- `-i audio2.mp3`: Second input audio file.
- `-filter_complex`: Indicates the beginning of a complex filter.
- `[0:a][1:a]`: Represents the audio streams from the first and second input respectively.
- `amix=inputs=2`: Combines two audio inputs into one output.
- `output.mp3`: Name of the resulting mixed audio file.

---

### Advanced Example with Video
In a more complex scenario, you might want to overlay text on a video, while also including a secondary video.

1. **Command for Overlaying Text**:
   ```bash
   ffmpeg -i input.mp4 -i logo.png -filter_complex "[0:v][1:v]overlay=W-w-10:H-h-10" output.mp4
   ```

**Explanation of Each Component**:
- `-i input.mp4`: Main video input.
- `-i logo.png`: Image to overlay (like a logo).
- `overlay=W-w-10:H-h-10`: Places the overlay 10 pixels from the right and bottom of the frame.
- `output.mp4`: The final output video with the overlay applied.

---

### Additional Tips
- The filters within `-filter_complex` can be combined to produce even more complex effects. For example, you could mix audio and apply fade effects.
- You may need to use additional FFmpeg filters like `scale`, `fade`, `crop`, etc., depending on your needs.

### Practical Usage
When using `-filter_complex`, it is essential to have a good grasp of filter options available in FFmpeg. Make sure you reference the FFmpeg filter documentation for in-depth analysis and available filters.

---

## Example References:
For further study, you can refer to:
## https://ffmpeg.org/ffmpeg-filters.html
## https://trac.ffmpeg.org/wiki/AdvancedFilters
## http://ffmpeg.org/ffmpeg-all.html