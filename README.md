# NOAA APT Satellite Reception Using GNU Radio

## Project Overview
This project demonstrates how to process and decode NOAA APT (Automatic Picture Transmission) signals using GNU Radio and a pre-recorded .dat IQ sample file.
The signal is processed to generate .wav audio files, and then converted into satellite images using the Open Weather Community platform.

This project is based on gobelc/sdr-apt, with modifications to focus only on using the AptRx-data.grc flowgraph and external decoding.

## What We Used
- Ubuntu Linux (VirtualBox environment)
- GNU Radio Companion 3.10.12.0
- NOAA satellite .dat IQ file
- Open Weather Community for image decoding

## Setup Instructions

### Install the necessary dependencies:
```
sudo apt install gnuradio python3-numpy python3-scipy python3-pil
```

### Clone the project:
```
git clone https://github.com/hagitdahan/AptRx-data.git
cd AptRx-data
```

### Download the sample NOAA .dat file:
Download from: https://iie.fing.edu.uy/investigacion/grupos/artes-old/noaa/2019.01.05.16.36.27.dat

### Open the flowgraph:
1. Open `gnuradio/AptRx-data.grc` in GNU Radio Companion.
2. In the File Source block, set the path to the .dat file you downloaded.
3. Adjust output directories if needed.
4. Run the flowgraph to generate .wav files.
5. Upload the resulting .wav file from `wavfm_demod/` to Open Weather Community to decode the satellite image.

## Folder Structure
```
AptRx-data/
│
├── gnuradio/
│   ├── AptRx-data.grc     # Modified GNU Radio flowgraph
│   └── AptRxData.py       # Auto-generated Python script
│
├── wavfm_demod/           # Output FM demodulated WAV files (for decoding)
├── wavam_demod/           # Output AM demodulated WAV files (for analysis)
│
├── images/
│   ├── FlowGraph.png      # Screenshot of the flowgraph
│   └── DecodedImage.png   # Example of decoded satellite image
```

## Processing Outputs
When running the project, three .wav files are generated:

| Output Folder | Purpose | Usage |
|---------------|---------|-------|
| wavfm_demod/ | FM demodulated audio containing the APT image data | This is the file to use for decoding the satellite image |
| wavam_demod/ | AM envelope of the signal for signal strength analysis | Not needed for image decoding |
| wavam_demod/ (20800 Hz) | AM envelope resampled to 20800 Hz | Not needed for image decoding |

**Important:**
Only the .wav file from the `wavfm_demod/` folder should be uploaded to Open Weather to decode the satellite image.

## Block Explanation

| Block | Description |
|-------|-------------|
| File Source | Reads the .dat IQ recording of the satellite signal |
| Throttle | Controls data flow to simulate real-time processing |
| WFM Receiver | Demodulates the Wideband FM APT signal |
| Multiply Const | Adjusts audio gain |
| Rational Resampler | Resamples the audio to match decoder expectations |
| Hilbert Transform | Converts real signal to analytic signal for envelope detection |
| Complex to Magnitude | Extracts signal envelope |
| Low Pass Filter | Removes high-frequency noise from the demodulated signal |
| Moving Average | Smooths the signal |
| Delay | Aligns demodulated data with original data flow |
| Keep One in N | Downsamples signal for analysis |
| WAV File Sink | Saves the FM demodulated or AM demodulated signals into .wav files |
| Audio Sink | Plays audio signal (optional) |
| QT GUI Sinks (FFT, Waterfall, Time) | Displays live signal visualizations during processing |

## Visual Overview

### Flowgraph Diagram
![Flowgraph](https://github.com/hagitdahan/AptRx-data/blob/main/images/FlowGraph.png)

### Example of Decoded NOAA Satellite Image
![Decoded APT Image](https://github.com/hagitdahan/AptRx-data/blob/main/images/Screenshot%20from%202025-04-29%2013-50-07.png)

## Notes
- We did not use RTL-SDR hardware; a pre-recorded .dat IQ sample was used.
- We focused only on signal processing and FM demodulation.
- We used Open Weather Community for image decoding.
- Only the FM demodulated audio is used for decoding the APT image.

## Credits
- Original repository: [gobelc/sdr-apt](https://github.com/gobelc/sdr-apt)
- APT Image decoding: [Open Weather Community](https://apt.sualsoft.com/)
