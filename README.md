# Pilot Readback Corpus

A **synthetic** reference corpus of Air Traffic Control (ATC) pilot–controller exchanges in standard ICAO radiotelephony phraseology, intended as ground truth for evaluating the output of **LLM-based pilot and controller agents**.

> ⚠️ **Synthetic data.** All exchanges are author-written following ICAO Doc 4444 / Annex 10 phraseology. They are **not** transcriptions of real ATC recordings and contain no real flight identifiers, clearances, or operational data.

Also available on the Hugging Face Hub: [`santiisoutoo/pilot-readback-corpus`](https://huggingface.co/datasets/santiisoutoo/pilot-readback-corpus).

## Overview

The corpus covers three operational phases of a departure sequence — Delivery, Ground, and Tower — with 100 dialogue exchanges per phase. Each exchange pairs a controller transmission with the corresponding pilot readback that complies with ICAO phraseology.

## Intended Use

This dataset is designed as a **reference for evaluating pilot/controller agents** (LLM-based or otherwise) that generate or respond to ATC transmissions. Typical use cases:

- Score an agent's readback against the reference for **phraseology compliance** (callsign placement, readback of mandatory elements: squawk, runway, altitude, frequency, QNH…).
- Use the reference as a target for **content/semantic** scoring (structured field extraction, embedding similarity, LLM-as-judge rubrics).
- As prompts/completions for fine-tuning conversational ATC agents.

> ❌ **Not an ASR benchmark.** Despite the legacy `corpus_wer_*` filename prefix, this dataset is **not** suitable for Word Error Rate evaluation. WER penalises semantically equivalent variants of the same readback (different element orders, abbreviations, "decimal" vs "point", optional callsign tags), which inflates error rates even for operationally correct outputs.

## Structure

```
pilot-readback-corpus/
├── DEL/
│   ├── corpus_wer_del.txt    # Human-readable transcript format
│   └── corpus_wer_del.jsonl  # Structured rows (id, controller, pilot)
├── GND/
│   ├── corpus_wer_gnd.txt
│   └── corpus_wer_gnd.jsonl
└── TWR/
    ├── corpus_wer_twr.txt
    └── corpus_wer_twr.jsonl
```

| Phase | Files | Exchanges |
|-------|-------|-----------|
| DEL – Delivery | `DEL/corpus_wer_del.{txt,jsonl}` | 100 |
| GND – Ground   | `GND/corpus_wer_gnd.{txt,jsonl}` | 100 |
| TWR – Tower    | `TWR/corpus_wer_twr.{txt,jsonl}` | 100 |

## Format

The `.txt` files keep the original human-readable layout. Each exchange has an identifier, an ATC transmission (`>`), and a pilot readback (`<`):

```
[del_001]
> Ryanair four seven three, cleared to Dublin via the BELEN one GOLF departure, initial climb five thousand feet, squawk two three four one, QNH one zero one three, runway three two left.
< Cleared to Dublin via BELEN one GOLF, initial climb five thousand, squawk two three four one, QNH one zero one three, runway three two left, Ryanair four seven three.
```

The `.jsonl` files contain the same data as structured rows for easier programmatic use:

| Field | Description |
|-------|-------------|
| `id` | Unique identifier (e.g. `del_001`) |
| `controller` | ATC controller transmission |
| `pilot` | Pilot readback |

## Usage

Load directly from the Hugging Face Hub:

```python
from datasets import load_dataset

ds = load_dataset("santiisoutoo/pilot-readback-corpus")
print(ds["del"][0])
```

Or read the JSONL files directly:

```python
import json
from pathlib import Path

rows = [json.loads(line) for line in Path("DEL/corpus_wer_del.jsonl").read_text(encoding="utf-8").splitlines()]
```

## Limitations and Considerations

- **Synthetic, single author.** All exchanges were written by one author following ICAO phraseology. They reflect a single interpretation of standard procedures and do not capture the variability of real ATC operations.
- **English only.** No localised or accented variants.
- **Departure-only.** Approach, en-route, and arrival phases are not covered; the corpus represents only the outbound part of a flight (clearance delivery → ground taxi → takeoff).
- **No phonetic noise.** Exchanges are clean text; they do not model ASR errors, audio artefacts, or non-native pronunciation.
- **Limited diversity.** Callsigns, airports, runways, SIDs, and squawks are drawn from a small pool and are not statistically representative of real traffic.

## Project Context

This corpus is part of the **AIrport** TFG project — an ATC training simulator for X-Plane 12 that uses LLM agents to play the roles of pilot and controller. The corpus serves as ground truth for evaluating those agents' outputs against standard ICAO phraseology.

## License

[CC BY 4.0](LICENSE) — Creative Commons Attribution 4.0 International
