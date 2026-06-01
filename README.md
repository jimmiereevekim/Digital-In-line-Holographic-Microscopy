# DIHM — Digital In-line Holographic Microscope

A low-cost, open-source digital in-line holographic microscope (DIHM) built on Raspberry Pi, designed for scientific imaging and embedded systems research. This project combines 3D-printed hardware with Python-based camera control to enable coherent light microscopy without the cost of commercial instruments.

---

## Overview

Digital In-line Holographic Microscopy (DIHM) is a lensless imaging technique that records interference patterns (holograms) between a reference beam and light scattered by a specimen. This repository provides everything needed to build and operate a Raspberry Pi-powered DIHM system, including control software, hardware schematics, and 3D-printable enclosure files.

The system was developed as part of an embedded systems research initiative and is intended as a reproducible reference implementation for lab and educational use.

---

## Features

- Automated image acquisition via Raspberry Pi Camera Module
- Configurable experimental parameters (pinhole size, wavelength, optical distances)
- Automatic timestamped session directories with parameter logging
- LED illumination control through GPIO
- Dual-capture workflow: object hologram + background reference frame
- Fully 3D-printable enclosure (STL files included)

---

## Repository Contents

| File | Description |
|---|---|
| `DIHM_Camera.py` | Main acquisition script — configures the camera, controls GPIO, and captures holograms |
| `Lower_Box.stl` | 3D-printable lower enclosure for the optical assembly |
| `Upper_Box.stl` | 3D-printable upper enclosure |
| `Pinhole_Holder.stl` | 3D-printable mount for the pinhole aperture |

---

## Hardware Requirements

- Raspberry Pi (any model with GPIO and Camera Module support)
- Raspberry Pi Camera Module (HQ recommended; supports up to 3280 × 2464 resolution)
- LED light source
- Pinhole aperture (default: 15 µm)
- 3D-printed enclosure (print files included)
- Jumper wires

---

## Software Requirements

- **OS:** Raspberry Pi OS (Debian-based Linux)
- **Language:** Python 3
- **Dependencies:**
  - `picamera` — Raspberry Pi camera interface
  - `RPi.GPIO` — GPIO pin control
  - Standard library: `os`, `errno`, `time`

Install dependencies:

```bash
pip install picamera RPi.GPIO
```

---

## Setup & Usage

### 1. Configure Experimental Parameters

Open `DIHM_Camera.py` and set the parameters for your optical configuration:

```python
d1 = 30       # Distance between pinhole and camera (mm)
d2 = 5.91     # Distance between object and camera (mm)
ph = 15       # Pinhole diameter (µm)
wl = 430      # Illumination wavelength (nm)
object = 'objectname'  # Sample identifier
```

### 2. Run the Acquisition Script

```bash
python3 DIHM_Camera.py
```

### 3. Capture Workflow

The script will:
1. Create a timestamped output directory on the Desktop
2. Write a `Parameters.txt` log file with all session metadata
3. Activate the LED via GPIO pin 17
4. Prompt for the **object hologram** — position your sample and press Enter
5. Prompt for the **background frame** — remove the sample and press Enter
6. Deactivate the LED

Captured images are saved as `object0.jpg` and `background0.jpg` in the session directory.

---

## Optical Configuration

| Parameter | Default | Notes |
|---|---|---|
| Pinhole–Camera distance (`d1`) | 30 mm | Adjust for desired magnification |
| Object–Camera distance (`d2`) | 5.91 mm | Determines field of view |
| Pinhole size (`ph`) | 15 µm | Smaller = greater spatial coherence |
| Wavelength (`wl`) | 430 nm | Blue LED recommended |
| Camera resolution | 3280 × 2464 | Full sensor resolution |
| ISO | 100 | Fixed for consistent exposure |

---

## Output Structure

Each session generates a directory named by timestamp:

```
~/Desktop/YY.MM.DD_HH.MM/
├── Parameters.txt       # Session metadata log
├── object0.jpg          # Hologram of the sample
└── background0.jpg      # Background reference frame
```

---

## Reconstruction

Raw holograms require numerical reconstruction to recover amplitude and phase images. Common approaches include angular spectrum propagation and Fresnel diffraction. Compatible reconstruction libraries:

- [HoloPy](https://holopy.readthedocs.io/) (Python)
- [FIJI/ImageJ](https://imagej.net/) with HologramJ plugin

---

## Topics

`raspberry-pi` · `embedded-python` · `linux` · `linux-kernel` · `holography` · `microscopy` · `dihm` · `colorimeter` · `open-hardware`

---

## License

This project is open source. See `LICENSE` for details.

---

## Author

**Jimmie Reeve Kim** — Embedded systems & scientific imaging research
