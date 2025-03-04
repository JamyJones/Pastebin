## Summary: Using the `loudnorm` filter in ffmpeg

---

To address the issue of volume variations in your audio, you can use the `loudnorm` filter in `ffmpeg`. The `loudnorm` filter is an EBU R128 loudness normalization filter that helps to balance the audio levels in your tracks.

1. **Description of `loudnorm` Filter**:
The `loudnorm` filter is used to normalize the loudness of audio tracks to a specific target loudness level. This ensures that the audio maintains a consistent volume throughout the entire track. The `loudnorm` filter is part of the ffmpeg package, so you don't need to install any additional modules.

---

2. **Integrating `loudnorm` Filter in the Script**:
In your Python script, you need to modify the `ffmpeg_command` to include the `loudnorm` filter. Here is an updated version of your script that integrates the `loudnorm` filter:

```python
import pysrt
import edge_tts
from pydub import AudioSegment
from datetime import timedelta
import re
import subprocess

def seconds_to_hhmmss(seconds):
    hours = seconds // 3600
    minutes = (seconds % 3600) // 60
    seconds = seconds % 60
    return f"{int(hours):02}:{int(minutes):02}:{int(seconds):02}"

VOICE = "en-US-GuyNeural"
srt_range_process=input()
begin_at,end_at,discardedTimeAfterSeek,start_time_original_audio,duration_full_audio=srt_range_process.split('-')
def clean_srt(input_file, output_file):
    with open(input_file, 'r', encoding='utf-8') as file:
        content = file.read()

    # Remove <font> tags and their contents
    cleaned_content = re.sub(r'<font[^>]*>|<\/font>', '', content)

    with open(output_file, 'w', encoding='utf-8') as file:
        file.write(cleaned_content)

clean_srt('transcript.en.srt', 'cleaned_transcript.srt')

# Load the .srt file
subs = pysrt.open('cleaned_transcript.srt')

# Helper function to convert timestamp to seconds
def timestamp_to_seconds(timestamp):
    return timestamp.hours * 3600 + timestamp.minutes * 60 + timestamp.seconds + timestamp.milliseconds / 1000.0

# Process each subtitle
speeches = []
speech_delay = []
delay_labels = []
count = 0
speech_count = 1
logs=open('logs.txt','a')
for sub in subs:
    if count < int(begin_at):
        count += 1
        continue
    if count > int(end_at):
        break

    start_time = timestamp_to_seconds(sub.start)
    start_time = start_time - int(discardedTimeAfterSeek)
    end_time = timestamp_to_seconds(sub.end)
    end_time = end_time - int(discardedTimeAfterSeek)
    duration = end_time - start_time

    # Convert subtitle text to speech
    tts = edge_tts.Communicate(sub.text, VOICE)
    tts.save_sync(f'temp{count}.mp3')
    # Use ffmpeg to mute the audio at the specified timestamps and overlay the new audio
    delay_ms = start_time * 1000  # Convert start time to milliseconds
    label_delay = f"[d{speech_count}]"
    delay_labels.append(label_delay)
    speeches.extend(['-i', f'temp{count}.mp3'])
    speech_delay.append(f'[{speech_count}:a]adelay={delay_ms}|{delay_ms},volume=3.0{label_delay};')
    print("Done: " + sub.text)
    message = f'\n the speech that says "{sub.text}" is to be inserted starting at {seconds_to_hhmmss(start_time)} and ended at {seconds_to_hhmmss(end_time)} making it play for {seconds_to_hhmmss(duration)} for which the audio is {seconds_to_hhmmss(int(duration_full_audio))} long'
    logs.write(message)
    count += 1
    speech_count += 1

# Construct the filter_complex string
filter_complex_str = f"[0:a]volume=1.5[base];" + ''.join(speech_delay) + f"[base]{''.join(delay_labels)}amix=inputs={len(delay_labels)+1}:duration=longest, loudnorm=I=-16:LRA=11:TP=-1.5[audio_out]"

# Complete ffmpeg command
ffmpeg_command = ['ffmpeg','-ss',f'{start_time_original_audio}' ,'-i', 'input1.opus'] + speeches + ['-filter_complex', "'"+filter_complex_str+"'", '-map', '[audio_out]', '-threads', '4','-t',f'{duration_full_audio}', 'output.mp3']
logs.close()
# Write the command to a shell script correctly
with open('run_ffmpeg.sh', 'a') as f:
    f.write(' '.join(ffmpeg_command))
```

---

### Explanation:
1. **`loudnorm` Filter Parameters**:
   - `I=-16`: Target integrated loudness level set to -16 LUFS.
   - `LRA=11`: Target loudness range set to 11 LU.
   - `TP=-1.5`: Target true peak level set to -1.5 dB.

---

2. **Integrating `loudnorm` in Filter Complex String**:
   - The `filter_complex_str` now includes the `loudnorm` filter at the end, which ensures that the entire audio output is normalized.
   - The `loudnorm` filter is appended to the filter chain after the `amix` filter using a comma `,` separator.

---

### Example:
Below is an example of how to use the `loudnorm` filter in the `ffmpeg` command:

```sh
ffmpeg -i input.mp3 -af loudnorm=I=-16:LRA=11:TP=-1.5 output.mp3
```

---

## References:
## https://ffmpeg.org/ffmpeg-filters.html#loudnorm ##

I hope this helps! If you have any other questions or need further assistance, feel free to ask.