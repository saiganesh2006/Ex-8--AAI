 <H3>ENTER YOUR NAME : D.B.V.SAI GANESH</H3>
<H3>ENTER YOUR REGISTER NO : 212223240025</H3>
<H3>EX. NO.8</H3>
<H3>DATE: 20-05-2026</H3>
<H1 ALIGN =CENTER>Implementation of Speech Recognition</H1>

## Aim:
To implement the conversion of live speech to text

## Algorithm:
<b>Step 1:</b> Import the speech_recognition library

<b>Step 2:</b> Initialize the Recognizer

<b>Step 3:</b> Create an instance of the Recognizer class, which will be used for recognizing speech

<b>Step 4:</b> Set the duration for audio capture

<b>Step 5:</b> Define a variable to specify the duration (in seconds) for which the program will capture audio from the microphone

<b>Step 6:</b> Display a message in the console to prompt the user to speak

<b>Step 7:</b> Capture audio from the default microphone

Step 9:</b> Use the default microphone as the audio source

<b>Step 10:</b> Record audio for the specified duration using the Recognizer instance

<b>Step 11:</b> Perform speech recognition with exceptional handling:<br>
•	Attempt to recognize speech from the captured audio using the Google Speech Recognition service<Br>
•	If successful, print the recognized text<Br>
•	Handle specific exceptions: If the recognition result is unknown or if there is an issue with the request to the Google Speech Recognition service, print corresponding error messages<Br>
•	A generic exception block captures any other unexpected errors<Br>

## Program:
```python
from google.colab import output
output.enable_custom_widget_manager()

!pip install SpeechRecognition

from IPython.display import Javascript, display
from google.colab import output
from base64 import b64decode
from pydub import AudioSegment
import speech_recognition as sr

# JavaScript to record audio
RECORD = """
async function recordAudio() {
  const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
  const recorder = new MediaRecorder(stream);

  let chunks = [];

  recorder.ondataavailable = e => chunks.push(e.data);

  recorder.start();

  await new Promise(resolve => setTimeout(resolve, 5000));

  recorder.stop();

  await new Promise(resolve => recorder.onstop = resolve);

  const blob = new Blob(chunks);

  const reader = new FileReader();
  reader.readAsDataURL(blob);

  return await new Promise(resolve => {
    reader.onloadend = () => resolve(reader.result);
  });
}
"""

display(Javascript(RECORD))

# Record audio
audio_data = output.eval_js("recordAudio()")

# Decode and save webm file
audio_bytes = b64decode(audio_data.split(',')[1])

with open("recorded_audio.webm", "wb") as f:
    f.write(audio_bytes)

# Convert webm to wav
sound = AudioSegment.from_file("recorded_audio.webm")
sound.export("recorded_audio.wav", format="wav")

# Speech recognition
recognizer = sr.Recognizer()

with sr.AudioFile("recorded_audio.wav") as source:
    audio = recognizer.record(source)

try:
    text = recognizer.recognize_google(audio)
    print("You said:", text)

except sr.UnknownValueError:
    print("Could not understand audio")

except sr.RequestError as e:
    print("Google API error:", e)

except Exception as e:
    print("Error:", e)
```

## Output:
<img width="347" height="41" alt="image" src="https://github.com/user-attachments/assets/687d158a-78f7-461d-a82b-a0f271994a78" />


## Result:
Thus, we have implemented the Python program that will transcribe the audio file in the file variable and print the transcribed text
