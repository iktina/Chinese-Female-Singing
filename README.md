# Chinese-Female-Singing

## Welcome
The service receives a midi file, text for singing and minimum significant time in seconds between two adjacent notes for pauses and uses them as an input for a trained model.
## What’s the point?
The service synthesizes a singing voice in Chinese based on the given text and notes. The service receives a midi file with notes to sing, the text to be sung and the minimum time in seconds to take into account pauses (the latter is optional). The service converts the input midi file, extracting information about notes, pauses between them and the duration of each note and pause, and then synthesizes the singing voice using machine learning methods.
## Model details:

## How does it work?
The user must provide the following inputs in order to start the service and get a response:
The user must provide the following input to start the service and receive a response:

1. MIDI file with single notes for each time period. (for details see the requirements for the midi file)
2. Text in the form of Chinese characters with pause tokens. (See information about pauses for more details)
3. Minimum time in seconds for a meaningful pause. (default 0.25 sec)

At the output, the user will receive an audio file in mp3 format.
### Midi file requirements

* Notes should not overlap, i.e. only the part that should be sung should be played. You can first get rid of unnecessary notes in any midi editor.
* Keep in mind that the number of notes and rests in the midi file must match the number of all hieroglyphs. Pauses of SP and AP are also considered. You can also check the number of notes and significant pauses in any midi editor.
* To avoid accidental pauses between notes, you can adjust the minimum duration for a pause. To do this, you need to change the value of normalize. It is recommended to put 0.25 seconds for it. If you want to keep all pauses, even the smallest ones, then set the value to 0.0.
* Don't forget to specify the path to save the output audio
* Good results can be obtained on the 3rd or 4th octave when singing

### Pause information

There are 2 types of pauses: AP and SP. AP is recommended to be used only at the beginning of singing because the breath is voiced. SP is a more natural pause. If you are not sure what you need, it is best to always use the SP pause.

## What to expect from this service?
