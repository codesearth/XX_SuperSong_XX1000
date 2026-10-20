# XX_SuperSong_XX1000

# Sonic Signatures: Audio Recognition Pipeline 🎵

**Team Members:**

Group 1: naman sambhav ujjwal vansh.
                  
Group 2: arnav ojas sayujya yuv.

Group 3: anchit aniket chinmay raghav.

## Project Overview
This project explores digital signal processing by building a Shazam-style song recognition pipeline from scratch. It takes a short, noisy audio clip, extracts spectral peaks into a constellation map, generates hash fingerprints, and identifies the song using time-offset histograms.

## Pipeline Architecture
* **Group 1:** Loads audio arrays, generates STFT spectrograms, and isolates local spectral peaks (constellation maps).
* **Group 2:** Pairs peaks to generate SHA-1 hash fingerprints and builds the time-offset voting histogram engine.
* **Group 3:** Torture-tests the recognizer against white noise, time-stretching, and pitch-shifting to evaluate robustness.

## How to Run This Project
The easiest way to run the pipeline is directly in Google Colab, which avoids any local installation issues.
# It is interactable and you can upload your own audio to find too!!

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/codesearth/XX_SuperSong_XX1000/blob/main/SuperSong1000.ipynb)

OR

 Click on the `.ipynb` notebook file in this repository.
 Click the **Open in Colab** button at the top of the file view.


 
1. Run Cell 1 to automatically download the 40-song database into the Colab environment.
2. Click **Runtime > Run all** to execute the audio extraction and launch the interactive testing dashboards.
