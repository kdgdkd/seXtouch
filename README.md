# seXtouch - X-touch Mini Sequencer

**seXtouch** will turn your Behringer X-touch mini into a 16 step sequencer. 

seXtouch is a json rules file for [MIDImod](https://github.com/kdgdkd/MIDImod), which runs on python. 




## Prerequisites

*   **Python 3.x**
*   **MIDImod script** (and its dependencies, e.g., `mido`, `prompt_toolkit`)
*   **Behringer X-Touch mini**
*   An external **MIDI clock source**, like a hardware clock, [MIDImaster](https://github.com/kdgdkd/MIDImaster)  or a DAW. It is works best with MIDImaster.
   

## Installation & Setup

0.  Load the bin templates for Layers A and B into the X-touch mini with the X-TOUCH Editor.
1.  Ensure you have [MIDImod](https://github.com/kdgdkd/MIDImod) set up and working.
2.  Copy the seXtouch JSON file into the `rules` directory used by MIDImod.
3.  Configure seXtouch's `device_alias` in the JSON files to match the MIDI port names of your clock source and your desired output. 
4.  Run MIDImod, selecting the desired seXtouch rule file via command-line arguments.

    ```bash
    python midimod.py sextouch
    ```

### About the master clock

seXtouch requires an external MIDI clock singal. External hardware clocks are better than software, but they are usually either master or slave clocks... If you use seXtouch with a master clock that receives transport signals, you will be able to start and top the sequencer directly from the X-touch mini.



#### Using seXtouch with MIDImaster

[MIDImaster](https://github.com/kdgdkd/MIDImaster)  is a rather crude python clock intended for testing. It will accept transport signals coming from the X-touch. For this to work, you will need to copy the seXtouch_CLOCK.json file into the MIDImaster rules folder. Then you can open MIDImaster using that rules file, and it will read input from the X-touch.  
On Windows, we will need two different virtual ports. I use midiLoops to create a CLOCK port (from MIDImaster to the X-touch) and a TPT port (from X-touch to MIDImaster). If you use different ones, you will need to update the names in the devices section in the jsons.   
With this implementation, you can send start, stop and set the BPM of the sequencer's clock directly from the X-touch mini.


#### Using seXtouch with any other master clock

You can configure any other master midi clock. You will need to update the device_alias section in the json rules file, so seXtouch knows which device to listen to. If you use a master clock that receives transport MIDI signals (start and stop), you can map them in the json (easy to find: these are the last two blocks in the file). 


## How to use seXtouch
The two rows of 16 buttons are the (up-to) 16 steps in the sequencer. By clicking on them, we mute/unmute the step. An iluminated button means that the step will generate a note when active. 
All other changes are done with the knobs. 


A. Pushing the first knob (and when loading seXtouch) starts "transposition mode". Moving the knobs on Layer A will change the transposition (the note) of the first 8 steps; Layer B changes the transpostion of steps 9-16 (the lower row).

B. Pushing the second button starts "velocity mode". The indicators blink. If you push the knob while being on Layer A, the knobs will control the velocity (the volume) of the first 8 steps; when you pùsh while being on Layer B, the knobs control the velocity of steps 9-16.

C. Pushing on the third and fourth button will transpose the full secuence down or up. This is aligned with the symbols on the lower row of buttons.

D. Pushing on the fifth knob will enter "config mode". The ring of the knob will iluminate. In this mode we can change global variables:  
1. First knob determines the step length. Base is 1/16, but it can be increased to 1/8, 1/4, 1/2, 1 and 2 bars per step  
2. Second knob determines the number of steps in the sequence, from 1 to 16  
3. Third knob selects the output channel, 1 to 16.  
4. Fourth does nothing yet.  
5. Fifth knob changes the direction, more or less. If the value is positive, it will move forwards. If it is negative, it will move backwards. If it is 3, it will play every third step, and so on.  
6. Sixth knob allows to choose between scales. Some scales are predefined in MIDImod, some directly on the json, feel free to tinker.  
7. Seventh knob is the global transposition.

E. The sixth push button sends a stop signal to the clock (if, as MIDImaster, can at a time be master clock and receive transport signals)
F. The seventh push button is to send a start signal to the clock.
G. The eight push button reinitalizes everything.

If you use MIDImaster, the fader controls the bpm. But you may want to change slightly the code, so it actually sends some kind of CC (say CC7 to control the volume) to the receiving gear.

## On another control surface
I would like to think that this project is easily adaptable for other MIDI control surfaces. The main challenge would be sending feedback to the control surface to manage the lights to show the advance of the step sequencer; this may work in very different ways depending on the device, but it is rather easy with the X-touch mini, and well documented in the manual.   
seXtouch's MIDI configuration for the X-touch Mini is very simple:
** The 16 buttons send notes 0 to 15 on both layers
** The 16 knobs send CCs 0-15 (8 per layer)
** The 16 push knobs send notes 36-51 (8 per layer)
** The fader sennds CC 127 on both layers, it is used to control the BPM
Note that the 16 buttons used to show the steps in the sequencer are toggle buttons, while the push knobs are momentary buttons. 

## Final say
Of course, all contributions super welcome. Enjoy. Do things.


