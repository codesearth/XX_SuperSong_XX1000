# SuperSong: Final Conclusions & Problem Statement Answers

This document directly addresses the core research questions posed in the project brief, comparing the Shazam-style peak-pair hash recognizer (Task 6) with the simple average spectral baseline (Task 5).

---

## Part 1: Audio Analysis (Group 1 Questions)

**1. What is the trade-off between time and frequency resolution? (Task 2)**
The trade-off is governed by the window size (`n_fft`) used in the Short-Time Fourier Transform (STFT). 
* **Small Window (e.g., 512 samples):** Provides high *time resolution*, meaning we can pinpoint exactly when a drum hit or transient occurs. However, it results in poor *frequency resolution*, smearing the exact pitch across multiple bins.
* **Large Window (e.g., 4096 samples):** Provides high *frequency resolution*, allowing us to perfectly isolate specific musical notes. However, it results in poor *time resolution*, smearing the exact moment the note was played.
* **Conclusion:** We used a middle ground (`n_fft=2048`) to get a balanced spectrogram capable of capturing both rhythm and melody.

**2. How do peak thresholds and minimum spacing distinguish useful peaks from clutter? (Task 3)**
Audio spectrograms are incredibly noisy. If we select every local maximum, we generate too many "clutter" peaks, exponentially increasing our database size and slowing down matching.
* **Thresholding (e.g., -40 dB):** Ensures we only look at the loudest, most prominent audio features, filtering out background noise and quiet instruments.
* **Minimum Spacing (Time/Freq Radii):** Prevents the algorithm from selecting dozens of peaks clustered around a single loud sustained note. It forces the algorithm to look for mathematically distinct "stars" to form a clean constellation map.

---

## Part 2: Robustness and Comparison (Group 3 Questions)

**3. How do query duration and starting position affect recognition? (Task 8)**
* **Shazam Method:** Extremely robust. Because it relies on localized peak-pair hashes, it can identify a song from a clip as short as 2 to 3 seconds. It also succeeds regardless of where the clip starts (intro, chorus, or outro).
* **Baseline Method:** Highly sensitive. It completely fails on short 2-second clips and fails on intros/outros. This is because a 2-second clip of a quiet guitar intro has an average frequency profile that looks absolutely nothing like the average profile of the full 3-minute song.

**4. How does adding different amounts of noise affect recognition? (Task 9)**
* **Shazam Method:** Highly resistant to noise. Even at 0 dB SNR (where the static noise is as loud as the music), the highest-energy musical peaks still poke above the noise floor. The algorithm only needs a small handful of these surviving peaks to form matching hashes and create a spike in the time-offset histogram.
* **Baseline Method:** Fails rapidly under noise. Additive White Gaussian Noise (AWGN) adds energy across all frequency bins equally. This flattens out the query clip's average frequency profile, ruining the Cosine Similarity calculation.

**5. How do the methods compare under time stretching and pitch shifting? (Task 10)**
This is where the two methods show their fundamental differences:
* **Time-Stretching:** The Baseline method *succeeds* because averaging the spectrum across time ignores the speed of the song. The Shazam method *fails* immediately; stretching time changes the time difference ($\Delta t$) between peaks, resulting in completely different SHA-1 hashes.
* **Pitch-Shifting:** The Baseline method degrades smoothly as the frequencies shift out of alignment. The Shazam method *fails* instantly; shifting the pitch moves the peaks into different frequency bins ($f_1$ and $f_2$). A single bin change in a cryptographic hash completely destroys the index match.

---

## Final Conclusion

The **Shazam-style Peak-Pair Hashing** pipeline is a vastly superior algorithm for real-world audio recognition compared to simple spectral averaging. By reducing audio to a sparse map of high-energy points and hashing their relative distances in time and frequency, the system achieves remarkable robustness against additive background noise, poor microphone quality, and ultra-short query durations. 

However, this exactness is a double-edged sword. Because the hashes rely on precise integer bin math, the Shazam method is entirely brittle to non-linear audio transformations like pitch-shifting (key changes) and time-stretching (speed changes). Despite this limitation, for identifying unaltered commercial music in noisy environments, the time-offset histogram approach remains an incredibly elegant and powerful application of Digital Signal Processing.
