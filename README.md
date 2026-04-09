# ASR-WFST-Viterbi — Automatic Speech Recognition System

WFST-based ASR system with custom Viterbi decoder and adaptive pruning · 81% WER reduction

---

## Overview

A complete Automatic Speech Recognition system built using a Weighted Finite-State Transducer (WFST) framework combined with a custom Viterbi decoding algorithm. The system maps speech to text using a pronunciation lexicon, HMM-based phone modelling, and a pre-trained TDNN-F neural network as the acoustic observation model.

Built as part of the Automatic Speech Recognition course at the University of Edinburgh.

---

## Results

| Configuration | WER | Decode Time (s) |
|--------------|-----|-----------------|
| Baseline | 11.18 | 2.18 |
| Tuned + Silence Modelling | 3.64 | 3.27 |
| Pruning (threshold=50) | **2.08** | **1.43** |
| Estimated Unigram | 3.56 | 3.99 |

**Best configuration achieved:**
- 81% reduction in Word Error Rate (11.18 → 2.08)
- 34% reduction in decoding time vs tuned system
- Evaluated on a custom dataset of 48 recorded audio files

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Language | Python |
| Framework | WFST (custom implementation) |
| Acoustic Model | Pre-trained TDNN-F neural network |
| Decoding | Custom Viterbi algorithm |
| Evaluation | WER (substitutions, deletions, insertions) |

---

## System Architecture

The system consists of three main components:

### 1. WFST Construction
- Pronunciation lexicon maps words to phone sequences
- Each phone modelled as a left-to-right HMM with 3 states
- Phone-level HMMs concatenated into word-level representations
- Word transitions connected to allow full sequence decoding

### 2. Acoustic Observation Model
- Pre-trained TDNN-F neural network
- Produces frame-level likelihood scores for each HMM state
- Scores converted to negative log-likelihoods for Viterbi decoding

### 3. Viterbi Decoder
- Custom implementation operating in the log domain
- Handles epsilon transitions separately
- Stores backpointers for full transcription recovery
- Tracks decoding time and forward computation count

---

## Experiments

### Key Parameters

| Parameter | Description |
|-----------|-------------|
| `continue_prob` | Word transition probability (loop penalty) |
| `self_loop_prob` | HMM state self-loop probability |
| `silence_modelling` | Optional silence HMM between words |
| `pruning_threshold` | Search space pruning cutoff |
| `unigram_probabilities` | Word-level decoding bias |

### Configurations Tested

| Task | cont_prob | self_loop | Silence | Pruning | Unigram |
|------|-----------|-----------|---------|---------|---------|
| Baseline | 1.0 | 0.1 | No | No | No |
| Loop Penalty | 0.1 | 0.1 | No | No | No |
| Self-loop Tuned | 0.1 | 0.4 | No | No | No |
| + Silence | 0.1 | 0.4 | Yes | No | No |
| + Unigram | 0.1 | 0.4 | Yes | No | Yes |
| + Pruning | 0.1 | 0.4 | Yes | 50 | No |
| Adaptive (Best) | 0.1 | 0.4 | Yes | Adaptive | Yes |

---

## Dataset

- 48 custom recorded audio files with reference transcriptions
- Audio converted to 16 kHz, mono, 16-bit PCM
- Transcripts normalised (lowercase, punctuation removed)
- Lexicon aligned with transcripts to eliminate OOV words

---

## Repository Structure
```
├── assignment.ipynb        # Main notebook — full pipeline
├── observation_model.py    # TDNN-F acoustic model interface
├── wer.py                  # Word Error Rate evaluation
├── lexicon.txt             # Pronunciation lexicon
├── phonelist.txt           # Phone inventory
├── recordings/             # Raw audio files
├── recordings_fixed/       # Preprocessed audio (16kHz mono)
├── tmp.dot                 # WFST graph (Graphviz)
├── tmp.png                 # WFST visualization
└── README.md
```

---

## Running the System

Open `assignment.ipynb` in Jupyter and run cells sequentially. The notebook covers:

1. WFST construction from lexicon and phonelist
2. Acoustic score extraction via observation model
3. Viterbi decoding with configurable parameters
4. WER evaluation and results analysis

---

## Authors

- **Omar Abdelhady** — [@3-bhd](https://github.com/3-bhd)

University of Edinburgh — Automatic Speech Recognition Course
