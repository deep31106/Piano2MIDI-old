# Piano2MIDI

A local, piano-focused audio-to-MIDI transcription project built with Python and machine learning.

The goal is to convert piano recordings into usable MIDI files while prioritizing transcription accuracy, local execution, and a modular architecture.

## Current Status

**Phase 5 — Pretrained Model Evaluation and Integration**

- Evaluated the Transkun pretrained piano transcription model.
- Benchmarked three 20-second excerpts from a MAESTRO-derived recording.
- Implemented a replaceable Transkun command-line backend.
- Added automated tests for backend success and failure cases.
- Verified that the integrated backend preserves all 95 note events in the initial test excerpt.

Benchmark scores are preliminary and come from excerpts of a single recording. They should not be interpreted as general accuracy guarantees.


## Project Structure

```text
piano2midi/
├── notebooks/
│   ├── phase_0_setup.ipynb
│   ├── phase_1_audio.ipynb
│   ├── phase_2_midi.ipynb
│   ├── phase_3_ground_truth_evaluation.ipynb
│   ├── phase_4_dsp_baseline.ipynb
│   └── phase_5_pretrained_model.ipynb
├── scripts/
│   └── benchmark_transkun.py
├── tests/
│   └── test_transkun_backend.py
└── src/
    └── piano2midi/
        ├── evaluation/
        │   └── metrics.py
        ├── transcription/
        │   └── transkun_backend.py
        └── __init__.py
```


## Requirements

- Python 3.12
- Git
- FFmpeg for preparing audio excerpts and future audio conversion workflows
- Transkun and its required dependencies in a separate Python environment
- A CUDA-compatible NVIDIA GPU for GPU inference; CPU inference may also be possible but can be slower

## Transkun Backend

The backend invokes Transkun through its command-line executable rather than importing Transkun into the main package environment.

This isolates the model's dependencies and makes it easier to replace the transcription backend later.

The backend accepts an audio path and output MIDI path and returns the generated MIDI path on success. It reports errors when the input is missing, the executable cannot be launched, transcription fails, or the output MIDI is missing.

Supported input extensions in the current adapter are WAV, MP3, FLAC, and OGG. MP4 audio extraction is not yet implemented.

## Development

From the repository root, run the automated backend tests with:

```bash
PYTHONPATH=src python -m pytest src/piano2midi/tests/test_transkun_backend.py -v
```

The tests use mocked Transkun calls to verify backend behavior without running neural-network inference.

## Current Limitations

- The user-facing CLI has not yet been implemented.
- MP4 audio extraction is not yet implemented.
- Duration correction and other transcription post-processing remain future work.
- Multi-song benchmarking and fresh-kernel notebook validation remain to be completed.
- The optional piano-roll viewer is planned only after the core transcription workflow is reliable.

## Project Goal

The intended end-user workflow is:

```bash
piano2midi song.mp3
```

producing a corresponding MIDI file locally, without requiring a paid hosted transcription service.

This command is the target interface; it is not yet available in the current implementation.