## Summary: Using the `loudnorm` Filter in FFmpeg

---

### Explanation:

**1. Introduction to `loudnorm` Filter:**
The `loudnorm` filter in FFmpeg is used for loudness normalization according to the EBU R128 standard. This filter is particularly useful for ensuring consistent loudness levels across different audio files.

---

**2. Basic Usage:**
To apply the `loudnorm` filter, you can use the following command:
```bash
ffmpeg -i input.wav -filter:a loudnorm output.wav
```
This command takes an input audio file (`input.wav`), applies the `loudnorm` filter, and outputs the normalized audio to `output.wav`.

---

**3. Two-Pass Normalization:**
For more accurate results, it's recommended to use the `loudnorm` filter in a two-pass mode. The first pass analyzes the audio properties, and the second pass applies the normalization based on the analysis.

**First Pass:**
```bash
ffmpeg -i input.wav -af loudnorm=I=-23:TP=-2:LRA=7:print_format=json -f null -
```
This command analyzes the input audio and prints the measured values in JSON format.

**Second Pass:**
```bash
ffmpeg -i input.wav -af loudnorm=I=-23:TP=-2:LRA=7:measured_I=-23:measured_TP=-2:measured_LRA=7:measured_thresh=-34.02:offset=-0.23:linear=true:print_format=summary output.wav
```
In the second pass, you use the measured values from the first pass to apply the normalization.

---

**4. Explanation of Parameters:**
- `I`: Integrated loudness target (in LUFS).
- `TP`: True peak target (in dBTP).
- `LRA`: Loudness range target (in LU).
- `measured_I`, `measured_TP`, `measured_LRA`, `measured_thresh`, `offset`: Values obtained from the first pass.
- `linear`: Enables linear normalization.

---

### Example:

**Example Command:**
```bash
ffmpeg -i input.wav -af loudnorm=I=-23:TP=-2:LRA=7:print_format=json -f null -
```
**Explanation:**
- `-i input.wav`: Specifies the input file.
- `-af loudnorm=I=-23:TP=-2:LRA=7:print_format=json`: Applies the `loudnorm` filter with specified parameters and prints the output in JSON format.
- `-f null -`: Discards the output, as this is only for analysis.

**Second Pass Example:**
```bash
ffmpeg -i input.wav -af loudnorm=I=-23:TP=-2:LRA=7:measured_I=-23:measured_TP=-2:measured_LRA=7:measured_thresh=-34.02:offset=-0.23:linear=true:print_format=summary output.wav
```
**Explanation:**
- `-i input.wav`: Specifies the input file.
- `-af loudnorm=I=-23:TP=-2:LRA=7:measured_I=-23:measured_TP=-2:measured_LRA=7:measured_thresh=-34.02:offset=-0.23:linear=true:print_format=summary`: Applies the `loudnorm` filter with measured values from the first pass.
- `output.wav`: Specifies the output file.

---

### References:
## https://trac.ffmpeg.org/wiki/AudioVolume ##
## https://superuser.com/questions/323119/how-can-i-normalize-audio-using-ffmpeg ##
## https://wiki.tnonline.net/w/Blog/Audio_normalization_with_FFmpeg ##

---

I hope this helps! If you have any more questions or need further clarification, feel free to ask.