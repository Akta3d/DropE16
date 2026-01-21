# Changelog

## [0.4] - 2026-01-21
- Save selected page color
- Add a section to the readme: How to update the layout without losing MIDI mapping
- Avoid value jumps during overwriting a loopback

## [0.3] - 2026-01-11
- Add record / play feature
- Add blink on BPM Led

## [0.2] - 2026-01-10
- Add DropE16.tosc file
- Add ChangeLog
- Improve Save / Load from logs
  - During load, log if component not found
  - Save more components
    - Pages name and color
    - Save and load Radials configuration
      - Radials: color
      - Labels: Text and color

## [0.1] - 2026-01-07
First commit

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

