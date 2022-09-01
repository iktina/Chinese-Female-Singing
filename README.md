# Chinese-Female-Singing

## Welcome
The service receives a midi file, text for singing and minimum significant time in seconds between two adjacent notes for pauses and uses them as an input for a trained model.

## What’s the point?
The service synthesizes a singing voice in Chinese based on the given text and notes. The service receives a midi file with notes to sing, the text to be sung and the minimum time in seconds to take into account pauses (the latter is optional). The service converts the input midi file, extracting information about notes, pauses between them and the duration of each note and pause, and then synthesizes the singing voice using machine learning methods.

### Input:

1. Singing text with pause tokens (if there are significant pauses in the midi file). Example: 

> SP祝你生日快乐SP祝你生日快乐

3. MIDI file with single notes. Example: 'Happy-Birthday.mid'

### Output:

The service returns the string dson. It contains information about the frequency of the output audio file (22050 Hz always) and bytes.
Example: 

> {"frequency": "22050", "bytes": "b'U5D0wFdXEcH9oCrBnIs8wRdu4sAxR+XA460KwGbkDD75nAM/V5k0v0u2GsBM0z3AN'"}

"b'U5D0wFdXEcH9oCrBnIs8wRdu4sAxR+XA460KwGbkDD75nAM/V5k0v0u2GsBM0z3AN'" - string of bytes
 
The output is a string in the form of a dictionary. It contains the frequency of the audio and the bytes for the audio waveform.
These bytes were received as follows:

> bytes = base64.b64encode(au)

The client has to receive one mono 22050 Hz audio.

Python code example how to convert a json string to the audio file:

> json = eval(result.output_audio)
> 
> s = int(json["frequency"])
> 
> bytess = str.encode(json["bytes"][2:-1])
> 
> wavfile.write("output_servise_ZH_singing.wav", s, np.frombuffer(base64.b64decode(bytess), dtype=np.float32).astype(np.int16))


## Important information

For the service to work correctly, you need to make sure that the number of notes + the number of pauses in the midi file is exactly the number of hieroglyphs, taking into account the pause tokens.

## Model details:

First, data is preprocessed on the server side, where notes and information about their duration and pauses between are extracted. The Chinese characters are then converted into phonemes. Thus, there will be 1 note for each phoneme (and for a pause in the text, there is a pause in the audio). Then this information is fed to the input of the neural model. We use DiffSinger, an acoustic model for SVS based on the diffusion probabilistic model. DiffSinger is a parameterized Markov chain that iteratively converts the noise into mel-spectrogram conditioned on the music score. To further improve voice quality and speed up output, a shallow diffusion mechanism is used.

## How does it work?
The user must provide the following inputs in order to start the service and get a response:

1. MIDI file with single notes for each time period. (for details see the requirements for the midi file)
2. Text in the form of Chinese characters with pause tokens. (See information about pauses for more details)
3. Minimum time in seconds for a meaningful pause. (default 0.25 sec)

At the output, the user will receive an audio file in wav format.
### Midi file requirements

* Notes should not overlap, i.e. only the part that should be sung should be played. You can first get rid of unnecessary notes in any midi editor.
* Keep in mind that the number of notes and rests in the midi file must match the number of all hieroglyphs. Pauses of SP and AP are also considered. You can also check the number of notes and significant pauses in any midi editor.
* To avoid accidental pauses between notes, you can adjust the minimum duration for a pause. To do this, you need to change the value of normalize. It is recommended to put 0.25 seconds for it. If you want to keep all pauses, even the smallest ones, then set the value to 0.0.
* Don't forget to specify the path to save the output audio
* Good results can be obtained on the 3rd or 4th octave when singing

### Information about pauses

There are 2 types of pauses: AP and SP. AP is recommended to be used only at the beginning of singing because the breath is voiced. SP is a more natural pause. If you are not sure what you need, it is best to always use the SP pause.

## What to expect from this service?

Audio track with singing in mono 22050 Hz.
