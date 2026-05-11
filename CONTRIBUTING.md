# Contributing

Thank you for helping expand the Pilot Readback Corpus! Contributions adding new ATC phases or exchanges are welcome.

## How to add a new corpus

### 1. Choose the right folder

| Phase | Folder | Description |
|-------|--------|-------------|
| Delivery | `del/` | IFR clearances, squawk, initial climb |
| Ground | `gnd/` | Pushback, taxi clearances |
| Tower | `twr/` | Takeoff/landing clearances |
| Approach | `acc/` | Approach clearances, vectors |
| Departure | `dep/` | Departure instructions, frequency changes |
| App (ATIS) | `app/` | ATIS broadcasts |

If your phase doesn't fit any existing folder, create a new one following the same naming convention.

### 2. File format

Name your file `corpus_wer_<phase>.txt` and follow this structure:

```
# Corpus WER — <PHASE> (<Description>)
# > ATC
# < Pilot

[<phase>_001]
> ATC transmission in plain English (numbers spelled out)
< Pilot readback

[<phase>_002]
> ...
< ...
```

**Rules:**
- Numbers must be spelled out: `three two left`, not `32L`
- Use standard ICAO phraseology
- Each exchange needs a unique identifier: `[<phase>_NNN]`
- ATC line starts with `>`, pilot readback with `<`

### 3. Submit

1. Fork this repository
2. Add your corpus file to the appropriate folder (remove the `.gitkeep` if present)
3. Open a Pull Request with a short description of the phase and number of exchanges added
