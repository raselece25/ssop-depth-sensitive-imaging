# 📡 SSOP Multi-Frequency Depth-Sensitive Imaging

[![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=flat-square&logo=mathworks&logoColor=white)](https://www.mathworks.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![NIH Funded](https://img.shields.io/badge/NIH-R01%20Funded-brightgreen?style=flat-square)](https://www.nih.gov/)

> Single-snapshot optical properties (SSOP) system using structured illumination and Fourier-domain demodulation for depth-resolved tissue optical property characterization — without sequential multi-image acquisitions.

**Part of NIH R01-funded research at the Biomedical Optical Imaging Laboratory (BOIL), Stony Brook University.**

---

## 📖 Overview

Traditional SFDI requires **multiple sequential image acquisitions** to extract tissue optical properties — incompatible with in vivo motion artifacts and real-time intraoperative use.

**Single-Snapshot Optical Properties (SSOP)** overcomes this by encoding multiple spatial frequencies into a **single image** using structured illumination patterns. Fourier-domain demodulation then separates frequency components, enabling simultaneous depth-sensitive optical property maps from one snapshot.

### SSOP vs. Traditional SFDI

| Feature | Traditional SFDI | SSOP (Ours) |
|---------|-----------------|-------------|
| Images required | 3–9 per frequency | **1 snapshot** |
| Motion sensitivity | High | **Minimal** |
| Real-time capable | No | **Yes** |
| Depth sensitivity | Per-frequency | **Multi-depth simultaneous** |
| In vivo compatibility | Limited | **High** |

---

## ✨ Key Features

- **Single-snapshot acquisition** — full optical property maps from one image frame
- **Multi-frequency encoding** — simultaneous multi-spatial-frequency illumination in one pattern
- **Fourier-domain demodulation** — phase-based separation of frequency components
- **Depth-resolved characterization** — depth sensitivity controlled by spatial frequency
- **Motion-robust** — eliminates inter-frame motion artifacts in in vivo imaging
- **Real-time processing** — MATLAB and Python implementations optimized for speed
- **Tissue phantom validation** — validated against known optical property standards

---

## 🔬 How It Works

```
Structured Illumination Pattern (multi-frequency sinusoidal)
              │
              ▼
    Single Image Acquisition
              │
              ▼
    2D Fourier Transform
              │
         ┌────┴─────┐
         │          │
    DC component   AC components (f₁, f₂, f₃...)
    (diffuse)      (modulated at each spatial freq)
         │          │
         └────┬─────┘
              ▼
    Band-pass Filtering per Frequency
              │
              ▼
    Inverse Fourier Transform
              │
              ▼
    Phase Demodulation (Hilbert transform)
              │
              ▼
    AC/DC Reflectance Maps per Frequency
              │
              ▼
    Optical Property Extraction (μa, μs′) per depth layer
```

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| Core Processing | MATLAB, Python |
| Fourier Analysis | MATLAB FFT/IFFT, NumPy FFT |
| Phase Demodulation | Hilbert transform, lock-in demodulation |
| Pattern Generation | Custom structured illumination synthesis |
| Optical Property Inversion | Lookup table (LUT), diffusion model |
| Visualization | MATLAB plotting, Matplotlib |
| Validation | Tissue phantoms, Monte Carlo simulation |

---

## 🗂️ Repository Structure

```
ssop-depth-sensitive-imaging/
├── ssop_core/
│   ├── pattern_generation/
│   │   ├── multi_freq_pattern.m    # Multi-frequency sinusoidal pattern synthesis
│   │   └── pattern_calibration.m  # Projector calibration & gamma correction
│   ├── demodulation/
│   │   ├── fourier_demod.m         # 2D FFT-based frequency separation
│   │   ├── phase_demod.m           # Hilbert-transform phase demodulation
│   │   ├── bandpass_filter.m       # Frequency-selective band-pass filtering
│   │   └── ac_dc_extract.m         # AC and DC reflectance map extraction
│   ├── optical_properties/
│   │   ├── lut_solver.m            # Lookup table optical property inversion
│   │   ├── diffusion_model.m       # Analytical diffusion model fitting
│   │   └── depth_sensitivity.m     # Depth sensitivity analysis per frequency
│   └── utils/
│       ├── image_io.m              # Image I/O and preprocessing
│       └── noise_analysis.m        # SNR and noise characterization
├── python/
│   ├── ssop_demod.py               # Python port of demodulation pipeline
│   ├── fourier_analysis.py         # NumPy-based Fourier processing
│   └── visualization.py            # Results visualization
├── validation/
│   ├── phantom_experiments/        # Tissue phantom validation datasets
│   └── monte_carlo_comparison/     # Comparison against MC ground truth
├── notebooks/
│   ├── 01_pattern_design.ipynb
│   ├── 02_demodulation_demo.ipynb
│   └── 03_depth_sensitivity_analysis.ipynb
└── tests/
```

---

## 🚀 Getting Started

### Prerequisites

```
MATLAB >= R2020a (Signal Processing Toolbox, Image Processing Toolbox)
Python >= 3.8 (optional, for Python modules)
```

### MATLAB Quick Start

```matlab
% Generate multi-frequency illumination pattern
pattern = generate_multi_freq_pattern( ...
    'frequencies', [0.05, 0.10, 0.15, 0.20], ...  % mm^-1
    'image_size',  [512, 512] ...
);

% Demodulate single snapshot
[ac_maps, dc_map] = ssop_demodulate(snapshot, ...
    'frequencies', [0.05, 0.10, 0.15, 0.20], ...
    'filter_width', 0.02 ...
);

% Extract depth-resolved optical properties
[mu_a, mu_sp] = extract_optical_properties(ac_maps, dc_map, 'method', 'lut');

% Visualize
plot_depth_maps(mu_a, mu_sp, 'frequencies', [0.05, 0.10, 0.15, 0.20]);
```

### Python Quick Start

```python
from python.ssop_demod import SSOPDemodulator

demod = SSOPDemodulator(
    frequencies=[0.05, 0.10, 0.15, 0.20],
    image_size=(512, 512)
)
ac_maps, dc_map = demod.process('data/raw/snapshot_001.tif')
print(f"Extracted {len(ac_maps)} frequency-resolved AC maps")
```

---

## 📊 Depth Sensitivity

| Spatial Frequency (mm⁻¹) | Approx. Probing Depth | Tissue Layer |
|--------------------------|----------------------|--------------|
| 0.05 | ~3.0 mm | Deep (bulk tissue) |
| 0.10 | ~2.0 mm | Mid-depth |
| 0.15 | ~1.5 mm | Superficial-mid |
| 0.20 | ~1.0 mm | Superficial |

---

## 📚 Publications

1. **Ahmmed, R.**, Kluiszo, E., Sunar, U. (2026). *Quantitative Fluorescence Imaging of Chemophototherapy Drug Pharmacokinetics Using Laparoscopic SFDI*. **Int. J. Molecular Sciences**, Q1, IF 4.9. [DOI](https://doi.org/10.3390/ijms)
2. Kluiszo, E., **Ahmmed, R.**, Sunar, U. (2025). *Mesoscopic Fluorescence Imaging of Light-Triggered Chemotherapeutic Release in Cancer Spheroid Models*. **Pharmaceutics**, Q1, IF 5.5.

---

## 🏆 Funding

NIH R01-funded — National Cancer Institute, Stony Brook University (2023–2026).

---

## 👤 Author

**Rasel Ahmmed** — PhD Candidate, Biomedical Engineering, Stony Brook University
[LinkedIn](https://www.linkedin.com/in/raahmmed) · [Portfolio](https://raselece25.github.io) · [Email](mailto:rasel.ahmmed@stonybrook.edu)

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.
