## Summary: Audio Inconsistency in Volume with FFmpeg amix Filter <br>
---
**Explanation:**<br>
1. **Volume Normalization Issue**<br>
The `amix` filter in FFmpeg can cause inconsistencies in volume when mixing multiple audio inputs. This happens because the filter normalizes the volume of each input based on the number of active inputs. As inputs drop out, the volume of the remaining inputs increases, leading to inconsistent volume levels.<br>
---
2. **Duration Mismatch**<br>
When the input files have different durations, the `amix` filter may not handle the volume levels correctly. The volume of the first mixed stream may be lower, while the last one is higher. This is because the filter scales each input's volume by 1/n, where n is the number of active inputs.<br>
---
3. **Solution: Normalize Parameter**<br>
To address this issue, you can use the `normalize` parameter in the `amix` filter. This parameter turns off the constantly changing normalization, ensuring consistent volume levels across all inputs. For example, you can modify your `amix` filter string to include `normalize=0`.<br>
---
**Example:**<br>
Here's an example command to mix multiple audio inputs with consistent volume levels using the `normalize` parameter:<br>
```bash
ffmpeg -i input1.mp3 -i input2.mp3 -i input3.mp3 -filter_complex "[0:a]adelay=1000|1000[a0]; [1:a]adelay=2000|2000[a1]; [2:a]adelay=3000|3000[a2]; [a0][a1][a2]amix=inputs=3:normalize=0" -c:a output.mp3
```
In this command:
- `adelay` is used to delay the start of each input to align them.
- `amix=inputs=3:normalize=0` ensures that the volume levels remain consistent across all inputs.<br>
---
**References:**<br>
## https://stackoverflow.com/questions/35509147/ffmpeg-amix-filter-volume-issue-with-inputs-of-different-duration ##<br>
## https://www.reddit.com/r/ffmpeg/comments/d63fki/preventing_amixs_volume_normalization_issue/ ##<br>