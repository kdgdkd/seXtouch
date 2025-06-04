# seXtouch - Advanced MIDI Control Surface Mapping for MIDImod

**seXtouch** is a collection of powerful and flexible JSON rule sets designed to be used with the [MIDImod](https://github.com/your_username/midimod) MIDI processing script. It allows you to transform a standard MIDI control surface (like a Behringer X-TOUCH or similar) into a highly customized and dynamic MIDI controller.

With seXtouch rules, you can go far beyond simple 1-to-1 MIDI mappings. Create multiple layers of controls, implement toggles, stepped parameters, conditional logic, and much more, all managed by the MIDImod engine.

## Key Features

*   **Dynamic & Conditional MIDI Mapping:** Remap notes to CCs, CCs to Program Changes, merge, split, filter, and transform MIDI messages based on complex conditions.
*   **Control Surface Layers/Modes (Versions):** Utilize MIDImod's "Version" system to create multiple independent mapping schemes for your controller. Switch between these "versions" on-the-fly using dedicated MIDI messages or keyboard shortcuts.
    *   For example, have one version where your faders control track volumes, and another where they control synth parameters, all switchable with a button press.
*   **Stateful Controls with Variables:** Leverage MIDImod's `var_#` user variables to create controls that remember their state. This is perfect for:
    *   **Toggles:** Press a button once to send one MIDI message, press it again to send another (or revert).
    *   **Stepped Parameters:** Cycle through a series of values with repeated button presses.
    *   **Custom Logic:** Implement complex interactions where the output depends on previous actions or the state of other controls.
*   **Expression-Based Control:** Define output values and version changes using mathematical and logical expressions, including references to `var_#` variables and incoming MIDI data.
*   **External Clock Synchronization:** seXtouch operates within the MIDImod environment. If MIDImod is connected to an external MIDI clock source (like **MIDIMaster**, a DAW, or other hardware/software sending MIDI Clock), seXtouch's MIDI message generation and version changes will be in sync with your master tempo.
    *   MIDImod receives and processes the MIDI clock; seXtouch rules then generate MIDI output that can be aligned with this clock (e.g., triggering sequenced parameter changes from your controller).
*   **Modular & Readable Configuration:** All logic is defined in human-readable JSON files, making it easy to understand, modify, and share your custom mappings.
*   **Designed for MIDImod:** seXtouch rules are specifically crafted to take full advantage of the features provided by the `midimod.py` script.

## How it Works

1.  **MIDImod Engine:** You run the `midimod.py` Python script. This script loads the seXtouch JSON rule files.
2.  **Control Surface Input:** Your MIDI control surface (e.g., X-TOUCH) sends standard MIDI messages (Notes, CCs, etc.) to an input port monitored by MIDImod.
3.  **seXtouch Rule Processing:**
    *   MIDImod intercepts these incoming messages.
    *   It evaluates them against the conditions defined in the active seXtouch `input_filter` rules and `version_map` rules.
    *   Based on these rules, MIDImod can:
        *   Change its active "Version" (mapping layer).
        *   Modify internal `var_#` variables.
        *   Generate one or more new MIDI messages.
4.  **Transformed MIDI Output:** The newly generated MIDI messages are sent out via a MIDImod output port to control your target devices (DAWs, synthesizers, other software).
5.  **Synchronization:** If MIDImod's input is receiving MIDI Clock from a source like MIDIMaster or your DAW, all operations within MIDImod, including those triggered by seXtouch rules, can be tempo-synced.

##What seXtouch Aims to Do

At its core, seXtouch, through MIDImod, allows you to:

*   **Maximize Controller Utility:** Get more functionality out of a limited number of physical controls by using versions and stateful logic.
*   **Create Custom Workflows:** Tailor your MIDI controller to behave exactly how you want for specific tasks or software.
*   **Bridge Devices:** Translate MIDI messages from your controller into formats understood by different target devices or software.
*   **Perform Live:** Dynamically change your controller's behavior during a performance.

## Prerequisites

*   **Python 3.x**
*   **MIDImod script** (and its dependencies, e.g., `mido`, `prompt_toolkit`)
*   A Behringer X-Touch mini.
*   An external MIDI clock source, like a hardware clock, MIDImaster or a DAW. It is works best with MIDImaster.
   

## Installation & Setup

1.  Ensure you have [MIDImod](https://github.com/kdgdkd/MIDImod) set up and working.
2.  Copy the seXtouch JSON file into the `rules` directory used by MIDImod.
3.  Configure MIDImod's `device_alias` in the JSON files to match the MIDI port names of your clock source and your desired output. 
4.  Run MIDImod, selecting the desired seXtouch rule file via command-line arguments.
    ```bash
    python midimod.py sextouch
    ```

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

E. The sixth push button is to send a stop signal to the clock (if, as MIDImaster, can at a time be master clock and receive transport signals)
F. The seventh push button is to send a start signal to the clock.
G. The eight push button reinitalizes everything.

If you use MIDImaster, the fader controls the bpm. But you may want to change slightly the code, so it actually sends some kind of CC (say CC7 to control the volume) to the receiving gear.




