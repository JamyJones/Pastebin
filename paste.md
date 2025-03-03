## Summary: Explanation of Using `loudnorm` Filter in FFmpeg to Normalize Audio

---

### Explanation:

**1**  
The `loudnorm` filter in FFmpeg is an implementation of the EBU R128 standard for loudness normalization. It ensures consistent audio levels by analyzing the loudness and applying gain to even out variations in volume. It is particularly useful for cases where your audio starts quietly and becomes loud or vice versa.

This filter operates in two passes:
- **Analysis Pass:** Calculates integrated loudness and true peak for the input audio.
- **Normalization Pass:** Adjusts the audio based on the results from the analysis pass.

---

**2**  
Here’s how you can incorporate the `loudnorm` filter into your FFmpeg command:

### **First Pass - Analysis**
Run the analysis pass to determine the loudness stats of your audio. For this, you use the `-f null` output option since you're only analyzing the input.

```bash
ffmpeg -i input_audio.mp3 -af loudnorm=I=-16:TP=-1.5:LRA=11:print_format=json -f null -
```

Explanation:
- `I=-16`: Target integrated loudness, measured in LUFS (loudness units relative to full scale).
- `TP=-1.5`: True peak level, measured in dBFS.
- `LRA=11`: Loudness range in LU.
- `print_format=json`: Outputs analysis results in JSON format.

The command will return stats like `input_i`, `input_tp`, and more. These stats will guide the second pass.

---

**3**  
### **Second Pass - Normalization**
Use the stats from the first pass to adjust the audio in the second pass. Here's an example:

```bash
ffmpeg -i input_audio.mp3 -af loudnorm=I=-16:TP=-1.5:LRA=11:measured_I=<value>:measured_TP=<value>:measured_LRA=<value>:measured_thresh=<value>:offset=<value>:linear=true output_audio.mp3
```

You replace `<value>` with the actual stats from the first pass.

Explanation:
- The `measured_*` options pass in values analyzed during the first pass.
- `offset`: Compensation applied to ensure the audio meets the specified parameters.
- `linear=true`: Ensures smooth and consistent gain adjustments.

---

**4**  
To integrate this into your Python script with FFmpeg, you can modify the section where the filter_complex string is constructed to include `loudnorm`. Here's an example code snippet:

```python
# Add loudness normalization filter in FFmpeg command
filter_complex_str = (
    f"[0:a]loudnorm=I=-16:TP=-1.5:LRA=11:print_format=json[normalized];"
    + ''.join(speech_delay)
    + f"[normalized]{''.join(delay_labels)}amix=inputs={len(delay_labels)+1}:duration=longest[audio_out]"
)
```

This ensures consistent loudness for the base audio track and speech overlays.

---

### Example:
To normalize an audio file `input.mp3` and output it as `normalized_output.mp3`, here's the complete command:

```bash
ffmpeg -i input.mp3 -af loudnorm=I=-16:TP=-1.5:LRA=11 normalized_output.mp3
```

---

### References:
## https://ffmpeg.org/ffmpeg-filters.html#loudnorm ##  
## https://trac.ffmpeg.org/wiki/AudioVolume ##