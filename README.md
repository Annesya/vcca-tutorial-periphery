# VCCA 2026 Tutorial: Computational Models of the Auditory Periphery

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Annesya/vcca-tutorial-periphery/blob/main/model_of_auditory_periphery_simplified_w_comments.ipynb)

This repository contains code for the hands-on session of the **VCCA 2026 tutorial on Computational Models of the Auditory Periphery**. The goal is to help participants build intuition for how sound is transformed by early stages of the auditory system, from the acoustic waveform to a simplified auditory-nerve representation.

The tutorial is designed for researchers, clinicians, engineers, and students who may come from different backgrounds but want a practical, computational view of peripheral auditory processing.

---

## What this tutorial demonstrates

The notebook walks through a simplified auditory-periphery model in stages:

1. **Input sound waveform**
   - Load an example speech-in-noise waveform.
   - Visualize the waveform and spectrogram.

2. **Cochlear frequency analysis**
   - Use a bank of gammatone filters to approximate frequency selectivity along the cochlea.
   - Each filter channel represents a different cochlear place or characteristic frequency.

3. **Inner hair cell transduction**
   - Apply half-wave rectification as a simple approximation to inner hair cell transduction.
   - This converts oscillatory basilar-membrane-like motion into a more neural-like nonnegative signal.

4. **Limits of phase locking**
   - Apply a low-pass filter to approximate the fact that auditory-nerve fibers do not preserve fine temporal phase equally well at all frequencies.

5. **Auditory-nerve rate-level functions**
   - Convert inner-hair-cell-like output into instantaneous firing rates.
   - Compare high-, medium-, and low-spontaneous-rate auditory nerve fiber types.

6. **Spike count simulation**
   - Generate stochastic spike counts from firing rates using a binomial spike generator.
   - Visualize the result as a simplified **nervegram**, a time-by-frequency representation of auditory-nerve activity.

---

## Repository contents

```text
vcca-tutorial-periphery/
├── model_of_auditory_periphery_simplified_w_comments.ipynb  # Main tutorial notebook
├── modules.py                                               # PyTorch modules for filters and neural transformations
├── filters.py                                               # FIR filter construction utilities
├── utils.py                                                 # Signal generation, plotting, and auditory-scale utilities
├── LICENSE                                                  # MIT License
└── .gitignore
```

---

## Getting started

### Option 1: Run in Google Colab

The easiest way to run the tutorial is through Google Colab:

1. Click the **Open in Colab** badge at the top of this README.
2. In Colab, select **Runtime > Run all** or step through the notebook one cell at a time.
3. Make sure the example audio file expected by the notebook is available at the path specified in the notebook:

```python
audio_file_path = "/content/example_speech_in_noise.wav"
```

If your audio file is stored somewhere else, update `audio_file_path` before running the notebook.

### Option 2: Run locally

Clone the repository:

```bash
git clone https://github.com/Annesya/vcca-tutorial-periphery.git
cd vcca-tutorial-periphery
```

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate  # macOS/Linux
# .venv\Scripts\activate   # Windows PowerShell
```

Install dependencies:

```bash
pip install numpy scipy matplotlib soundfile tqdm ipython jupyter soxr torch torchaudio
```

Start Jupyter:

```bash
jupyter notebook
```

Then open:

```text
model_of_auditory_periphery_simplified_w_comments.ipynb
```

---

## Expected input audio

The notebook expects an example audio file named:

```text
example_speech_in_noise.wav
```

In Colab, the current notebook path assumes:

```text
/content/example_speech_in_noise.wav
```

You can either:

- upload a file with that name into the Colab session, or
- change the `audio_file_path` variable in the notebook to point to your own `.wav` file.

For best results, use a monophonic or stereo `.wav` file with speech, noise, tones, or another sound that illustrates time-frequency structure.

---

## Main parameters to experiment with

The notebook is designed to be modified interactively. Useful parameters to change include:

```python
lowest_CF = 60
highest_CF = 8000
num_CFs = 50
filter_bandwidth = 1
phaselock_cutoff = 3000
```

These control the cochlear filterbank and inner-hair-cell low-pass stage.

Auditory nerve fiber parameters can also be changed:

```python
rate_spont_hsr = 70.0
rate_spont_msr = 4.0
rate_spont_lsr = 0.1

threshold_hsr = 0.0
threshold_msr = 12.0
threshold_lsr = 28.0

dynamic_range_hsr = 20.0
dynamic_range_msr = 40.0
dynamic_range_lsr = 80.0
```

Try changing these values to see how different assumptions about fiber threshold, dynamic range, and spontaneous firing rate alter the final nervegram.

---

## Conceptual model pipeline

At a high level, the notebook implements the following signal-processing chain:

```text
Sound waveform
   ↓
Gammatone filterbank
   ↓
Cochlear subband signals
   ↓
Half-wave rectification
   ↓
Inner-hair-cell low-pass filtering
   ↓
Auditory nerve rate-level function
   ↓
Stochastic spike-count generation
   ↓
Nervegram visualization
```

Each stage is intentionally simplified so that participants can see the relationship between auditory physiology and signal-processing operations.

---

## Notes for tutorial participants

You do not need to understand every implementation detail to benefit from the notebook. A useful way to follow the tutorial is to ask, at each stage:

- What biological structure or process is this stage trying to approximate?
- What signal-processing operation is being used as the approximation?
- What information is preserved, transformed, compressed, or lost?
- How do the model parameters affect the resulting auditory representation?

---

## Troubleshooting

### `ModuleNotFoundError: No module named 'soxr'`

Install the missing package:

```bash
pip install soxr
```

### `ModuleNotFoundError: No module named 'torchaudio'`

Install PyTorch and torchaudio:

```bash
pip install torch torchaudio
```

For GPU-specific installations, follow the official PyTorch installation instructions for your CUDA version.

### `RuntimeError` related to CUDA or GPU

The notebook should run on CPU. If CUDA is unavailable, the code automatically selects CPU:

```python
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
```

If you run into GPU-specific issues, restart the runtime and use CPU mode.

### Audio file not found

Check that the file exists at the path used by:

```python
audio_file_path = "/content/example_speech_in_noise.wav"
```

Update the path if needed.

---

## License

This project is released under the MIT License. See [`LICENSE`](LICENSE) for details.

---

## Acknowledgments

This material was prepared for the VCCA 2026 tutorial on computational models of the auditory periphery.

The codes shared in this tutorial have previously been developed and used for the following publications: 
`Saddler, M.R. and McDermott, J.H., 2024. Models optimized for real-world tasks reveal the task-dependent necessity of precise temporal coding in hearing. Nature Communications, 15(1), p.10590.`
`Banerjee, A., Saddler, M.R., Arenberg, J.G. and McDermott, J.H., 2025. A deep learning framework for understanding cochlear implants. bioRxiv.`

