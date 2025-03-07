## Pydub: Overview and Uses <br>
---<br>
Pydub is a powerful audio processing library for Python that simplifies the manipulation of audio files. It provides an easy-to-use interface to perform various audio operations without needing extensive knowledge of digital audio processing. Here’s a detailed overview of its uses:

1. **Audio File Manipulation** <br>
   Pydub allows you to read, manipulate, and export audio in various formats such as WAV, MP3, OGG, and more. The library leverages the capabilities of FFmpeg or libav for extensive support of audio formats.

   - **Common Tasks**:
        - **Loading Audio**: Pydub can read audio files into the program.
        ```python
        from pydub import AudioSegment
        audio = AudioSegment.from_file("example.mp3")
        ```
        This line imports the AudioSegment class from pydub and loads an audio file into the `audio` variable.

   - **Exporting Audio**:
        ```python
        audio.export("output.wav", format="wav")
        ```
        This line converts the audio stored in the `audio` variable into a WAV file format.

2. **Editing Audio** <br>
   A core component of Pydub is its editing capabilities, allowing for tasks such as slicing, concatenating, and adjusting properties.

   - **Slicing**:
        ```python
        segment = audio[1000:5000]  # Slice from 1s to 5s
        ```
        You can extract segments of the audio by specifying the start and end time in milliseconds.

   - **Concatenation**:
        ```python
        combined = audio1 + audio2  # Combine two audio segments
        ```

   - **Volume Adjustment**:
        ```python
        louder = audio + 10  # Increase volume by 10dB
        ```
        Pydub makes it easy to change the volume levels of audio clips.

3. **Applying Effects** <br>
   Pydub also supports various audio effects, such as fading, reversing, and more. 

   - **Fading**:
        ```python
        faded = audio.fade_in(2000)  # Fade in over the first 2 seconds
        ```
   - **Reversing**:
        ```python
        reversed_audio = audio.reverse()  # Reverse the audio
        ```

Example:
```python
from pydub import AudioSegment

# Load audio file
song = AudioSegment.from_file("your_song.mp3")  
# Increase the volume
louder_song = song + 5  
# Fade out the end
faded_song = louder_song.fade_out(3000)  
# Export the edited song
faded_song.export("edited_song.mp3", format="mp3")
```
In this example, Pydub is used to load a song, increase its volume, apply a fading effect, and finally export the edited song.

## References:
## https://coderslegacy.com/pydub-tutorial-audio-manipulation-in-python/ 
## https://github.com/jiaaro/pydub 
## https://medium.com/@danushidk507/pydub-afe485194295