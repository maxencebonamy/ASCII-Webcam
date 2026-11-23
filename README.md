# ASCII Webcam

Webcam feed rendered as ASCII art in the terminal.

The script reads frames from the default webcam, converts each one to grayscale and prints it as lines of characters, chosen by brightness. It is a small Python script for anyone who wants to see their camera as text.

## Table of contents

- [ASCII Webcam](#ascii-webcam)
	- [Table of contents](#table-of-contents)
	- [How it works](#how-it-works)
	- [Tech stack](#tech-stack)
	- [Repository structure](#repository-structure)
	- [Prerequisites](#prerequisites)
	- [Installation](#installation)
	- [Configuration](#configuration)
	- [Running locally](#running-locally)
	- [License](#license)

## How it works

1. **Capture**: OpenCV opens the default webcam (device `0`) and reads one frame at a time.
2. **Grayscale**: each frame is converted from BGR to grayscale.
3. **Sizing**: the output is `WIDTH` characters wide. The height is computed once, from the first frame, using the image ratio and a factor of 8/21 to compensate for the height of a character.
4. **Sampling**: the frame is split into a grid of `WIDTH` by height cells. The mean brightness of each cell is mapped to an index in the `ASCII` string, which gives one character.
5. **Output**: the grid is printed as text, then the script waits `1 / FPS` seconds before reading the next frame.
6. **Stop**: `Ctrl+C` ends the loop and releases the webcam.

## Tech stack

| Layer | Technologies |
| --- | --- |
| Language | Python 3 |
| Image capture and processing | OpenCV (`opencv-python`) |
| Computation | NumPy |

## Repository structure

```
.
├── demo/
│   └── ascii-webcam-demo.mp4   Screen recording of the output
├── config.py                   Settings: width, character ramp, FPS
├── main.py                     Webcam capture and ASCII conversion
├── LICENSE
└── README.md
```

Settings are plain module-level constants in `config.py`, imported with `from config import *`. There is no package structure, dependency file or test suite.

## Prerequisites

- [Python](https://www.python.org/) 3
- A webcam accessible as the default video device
- A terminal wide enough for `WIDTH` characters (200 by default)

## Installation

```bash
git clone git@github.com:maxencebonamy/ASCII-Webcam.git
cd ASCII-Webcam
```

The repository has no `requirements.txt`. Install the two libraries imported by `main.py`:

```bash
pip install opencv-python numpy
```

## Configuration

Edit the constants in `config.py`.

| Variable | Description | Example |
| --- | --- | --- |
| `WIDTH` | Width of the output in characters. The height follows from the image ratio | `200` |
| `ASCII` | Characters used for the rendering, as a single string. A frame's brightness selects the position in this string | `" .'`^\",:;Il!i><~+_-?][}{1)(\|\\/tfjrxnuvczXYUJCLQ0OZmwqpdbkhao*#MW&8%B@$"` |
| `FPS` | The script sleeps `1 / FPS` seconds between two frames | `60` |

## Running locally

```bash
python main.py
```

Stop the program with `Ctrl+C`. On macOS, the terminal application may need permission to use the camera.

## License

This project is licensed under the [MIT License](LICENSE).
