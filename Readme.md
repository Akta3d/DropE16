# Intro
DropE16 is a [TouchOSC](https://hexler.net/touchosc) MIDI Controller.

<img width="1469" height="969" alt="DropE16" src="/doc-assets/DropE16.png" />

## Demo
<img width="1468" height="974" alt="Interface" src="/doc-assets/Demo.gif" />


## Interface
<img width="2190" height="1165" alt="DropE16" src="/doc-assets/Interface.png" />

## Motivation

I was looking for a physical MIDI controller, but at the time I couldn’t afford one.  
After discovering TouchOSC, I decided to develop this project as a software alternative.

This project is largely inspired by:

- [OXI E16](https://oxiinstruments.com/oxi-e16)
- [Drop](https://www.neuzeit-instruments.com/Drop)

## Features

- Multi-page layout
- Radial MIDI CC controllers  
  - 16 radials per page  
  - MIDI channel and CC number configurable per radial
- Snapshots  
  - 8 snapshots per page  
  - Save current state of all radials of page 
  - Instant recall  
  - Morphing recall  
  - Delayed recall  
  - Reset
- Groups / Macros  
  - 2 macros per page  
  - Control multiple MIDI parameters with a single control  
  - Easy assignment of radials to macros  
  - Invert control values
- Save / Load  
  - Session automatically saved on exit  
  - Ability to load configurations from logs
- Easy to customize  
  - Add / remove pages  
  - Add / remove radials  
  - Add / remove snapshots  
  - Add / remove macros

## License

Creative Commons Attribution – ShareAlike 4.0 International (CC BY-SA 4.0)

You are free to share and adapt this material for any purpose, including commercial use, as long as appropriate credit is given.  
Any modified version of this TouchOSC layout must be distributed under the same CC BY-SA 4.0 license.

---

# How to Use

## Change MIDI CC

- Select a page
- Edit the container **`groupRadial`**
- Select a radial
- Change the MIDI message settings

## Use Radials

- Simply turn the radials to send MIDI CC values

## Use Snapshots

The 8 snapshot buttons allow you to store and recall the positions of the 16 radials.

Snapshot button appearance:

- Dashed outline: no snapshot stored  
- Solid outline: snapshot stored  
- Green button: last snapshot used  

### Save a Snapshot

- Press the **SAVE** button to enter *Save mode*
- Press the snapshot button where you want to store the current configuration  

This will overwrite any previously saved snapshot on that button.

### Reset a Snapshot

- Press the **RESET** button to enter *Reset mode*
- Press the snapshot button to clear its stored values

### Load a Snapshot

Snapshot loading behavior depends on the **Duration** and **Delay** radio buttons:

- **Duration**: morphs from the current radials position to the snapshot over the specified number of measures  
- **Delay**: waits the specified number of measures before loading the snapshot  

Examples:

- Instant load: `duration = 0`, `delay = 0`
- Morph over 1 measure, immediate start: `duration = 1`, `delay = 0`
- Instant load after 2 measures: `duration = 0`, `delay = 2`
- Wait 4 measures, then morph over 2 measures: `duration = 2`, `delay = 4`

<img width="1468" height="974" alt="Interface" src="/doc-assets/Snapshot.gif" />


## Use Macros

- Press the **Set** button of the desired macro  
  → Small buttons appear next to each radial
- Single-click a button to assign the radial to the macro
- Double-click a button to invert the control value

<img width="1468" height="974" alt="Interface" src="/doc-assets/Macro.gif" />


## Set BPM

The BPM in DropE16 is only used for snapshot **duration** and **delay**.  
TouchOSC does not support MIDI clock.

To set the BPM, tap the grey circle in the bottom-right corner.

## Save / Load

All parameters are automatically saved during the session.  
Before quitting TouchOSC, make sure to **save your template** to keep all settings for the next session.

## Save Multiple Configurations Using “Logs”

If you need to store multiple configurations, you can save them using a text editor.

### Save a Configuration

- Display the log view
- Open the settings dialog using the small red button (top-right)
- Click **Save**
- Copy the log starting from `## Page 1 ...`
- Paste it into a text file and save it

<img width="1468" height="973" alt="SaveLog" src="/doc-assets/SaveLog.png" />

### Restore a Configuration

- Open the editor and locate the component **`loadButton`**
- Open the script view on the right
- Paste the previously saved log string into the variable **`savedMessage`**
- Restart DropE16
- Open the settings dialog and click **Load**

<img width="1466" height="969" alt="LoadLog" src="/doc-assets/LoadLog.png" />

This process saves, for each page:

- All snapshot values
- Macros configuration

---

# How to Customize

## Add a Page

- Select **`pager1`**
- Add a new page
- Open the container **`pager1/x`** of the page you want to duplicate
- Copy all components
- Open **`pager1/new page`**
- Paste the components

## Add / Remove Radials

- Open the container **`groupRadial`** on the desired page
- Remove or copy/paste radial and label components
- Adjust colors and text
- Update MIDI settings as needed

## Add Snapshots

- Open the container **`groupSnapshot`** on the desired page
- Remove or copy/paste snapshot buttons

## Add Macros

- Copy and paste the group **`groupMacro1`**, then rename it
- Position the macro radial where desired
- Open **`groupSetMacro`** and reposition all buttons  
  *(this step requires careful placement)*

---

# Developer Notes

This section highlights where scripting is used.  
The goal was to minimize the number of scripted components.

Each similar component shares the same script, and **component names are important**.

## Script Locations

### Document

Main script.  
Actions are triggered via `notify / onReceiveNotify`.

When saving a snapshot, all radial values are stored in `snapshotButton.tag`.

### Snapshot Button

Handles calls to document functions using the notify mechanism, for these actions:

- Save
- Reset
- Play

### Radial Macro

Updates radials value when controlled by a macro.

### Macro Assignment Button

Manages:

- Single click / double click
- Value stored in `button.tag`:
  - `0` = disabled  
  - `1` = enabled  
  - `-1` = inverted  

### BPM Button

Handles tap tempo and LED blinking.  
The variables `bpm` and `bpmStartTime` are sent to the document to calculate snapshot duration and delay.

### Save Button

Outputs all component values to the log view.

### Load Button

Restores component values from a previously saved log
