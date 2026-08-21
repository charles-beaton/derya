# 🌊 Derya Audio Engine — Comprehensive Technical Architecture & DSP Manual

> **Repository**: `charles-beaton/derya`  
> **Application**: *Derya için Şiirler* — Private Encrypted Interactive Poetry Anthology  
> **Live Production URL**: [https://charles-beaton.github.io/derya/](https://charles-beaton.github.io/derya/)  
> **Master Generator Source**: [`build_app_js.js`](file:///c:/Users/charl/OneDrive/Documents/Derya/build_app_js.js) (compiles to `app.js`)  
> **Technology**: Pure **Web Audio API** — 100% Procedural Synthesis (Zero External Audio Files or Samples)  
> **Acoustic Design**: Generative Ocean Physics, Granular Sand Fizz, Just-Intonation Quartz Bowls, and 6Hz Theta Entrainment  
> **Last Updated**: August 2026

---

## 1. Overview & Generative Philosophy

Unlike conventional web applications that play static, repetitive loop files (e.g., MP3/WAV loops), the **Derya Audio Engine** synthesizes every acoustic event **live on-the-fly inside the client's browser** via the hardware Web Audio clock.

### Core Design Principles:
1. **Never Repeating (Infinite Non-Periodic Soundscape)**:
   Every wave swell, breaking crash, foam bubble burst, and receding backwash is procedurally generated using mathematical probability distributions (Poisson micro-bursts, Brownian leaky integration, and organic set clustering). No two waves are ever identical.
2. **Subconscious & Meditative Aesthetics**:
   The engine is tuned to a whisper-quiet, grounding ambient presence from the first unlock. It cushions reading and contemplation without acoustic fatigue or startling peaks.
3. **Physical Fluid Dynamics**:
   Models wave gathering, spatial peel trajectories along a beach, barrel cavity resonance, shoreline impact thud, and prolonged gravitational undertow.
4. **Zero Latency & Instant Response**:
   All sliders control Web Audio nodes directly in real-time via smooth `linearRampToValueAtTime` parameter transitions.
5. **Persistent Customization**:
   All adjustments are saved across sessions in `localStorage` and `document.cookie`.

---

## 2. Complete Web Audio Graph Architecture

```
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 1. PROCEDURAL SOURCES (RAM Buffers & Oscillators)                                                      │
├────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ • Stereo Brown Noise (Leaky Integration Buffer, 10s Seamless)                                          │
│ • White Noise Buffer (Uniform Stochastic Distribution, 5s)                                             │
│ • Granular Micro-Bubble Engine (Poisson-Distributed Pop Triggers λ=0.0035, 6s)                         │
│ • Quartz Crystal Sine Oscillators (432Hz • 540Hz • 648Hz Just Intonation)                              │
│ • Theta Binaural Sine Generators (Carrier: 100Hz Left, 106Hz Right)                                    │
│ • Sub-Bass Grounding Drone (55Hz / 110Hz Warmth)                                                       │
└────────────────────────────────────┬───────────────────────────────────────────────────────────────────┘
                                     │
                                     ▼
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 2. DSP SHAPING, PHYSICAL FILTERS & ENVELOPES                                                           │
├────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ [Layer 1: Background Ocean Bed]                                                                        │
│   Brown Noise ──▶ HPF (75Hz) ──▶ LPF (560Hz, Q:0.85) ──▶ Breathing LFO (1/12 Hz, ±45Hz) ──┐          │
│                                                                                             │          │
│ [Layer 2: Foreground Fluid Shoreline Waves]                                                 │          │
│   ├─▶ A. Swell Gathering: Brown Noise ──▶ HPF (75Hz) ──▶ Sweep LPF (160Hz ↗ 2400Hz) ────────┼──▶ [1]   │
│   ├─▶ B. Wave Barrel Cavity: White Noise ──▶ BPF (420Hz, Q:3.2) ──▶ Exp Envelope ───────────┼──▶ [2]   │
│   ├─▶ C. Breaking Shore Crash: White Noise ──▶ HPF (380Hz) ──▶ LPF (4800Hz, Q:0.707) ───────┼──▶ [3]   │
│   ├─▶ D. Granular Foam & Sand: Granular Buffer ──▶ HPF (2200Hz) ──▶ BPF (4600Hz, Q:0.8) ────┼──▶ [4]   │
│   ├─▶ E. Shore Impact Thud: White Noise ──▶ BPF (130Hz, Q:2.4) ──▶ 60ms Attack ─────────────┼──▶ [5]   │
│   └─▶ F. Meditative Undertow: White Noise ──▶ Downward Sweep BPF (1950Hz ↘ 580Hz, 5.2s) ───┼──▶ [6]   │
│                                                                                             │          │
│ [Layer 3: Harmonic Crystal Quartz Bowls]                                                     │          │
│   Quartz Sines (432/540/648Hz) ──▶ Independent Gain Envelopes (6.5s Slow Ramps) ───────────┼──▶ [7]   │
│                                                                                             │          │
│ [Layer 4: Binaural Theta Entrainment & Sub-Bass]                                             │          │
│   ├─▶ 6Hz Theta Beat: Osc L (100Hz) + Osc R (106Hz) ──▶ ChannelMergerNode(2) ───────────────┼──▶ [8]   │
│   └─▶ Sub-Bass Foundation: Dual Sines (55Hz/110Hz) ──▶ Low-End Saturation Gain ─────────────┘──▶ [9]   │
└────────────────────────────────────┬───────────────────────────────────────────────────────────────────┘
                                     │
                                     ▼
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 3. SUB-BUSES, TREBLE DAMPING & SPATIAL PANNING                                                         │
├────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ [1] Swell Audio + Background Ocean ──▶ wavesMasterGain ──────────────────────────────────────┐         │
│ [2] Barrel Formant ──────────────────▶ barrelMasterGain ─────────────────────────────────────┤         │
│ [3] Breaking Crash + [5] Thud ──┐                                                            │         │
│ [4] Granular Foam Fizz ─────────┴────▶ [🕊️ crashFoamTrebleFilter (LPF 400–12000Hz)] ────────┤         │
│                                        └─▶ Stereo Rolling Peel Panner (L ↔ R) ───────────────┤         │
│ [6] Undertow Backwash ───────────────▶ Inward Suction Panner (L/R ➔ Center) ──▶ undertowGain─┤         │
│ [7] Crystal Bowls ───────────────────▶ bowlMasterGain ───────────────────────────────────────┤         │
│ [8] Binaural Theta Channel ──────────▶ binauralGainNode (Isolated Direct Channel) ───────────┤         │
│ [9] Sub-Bass Drone ──────────────────▶ droneGain ────────────────────────────────────────────┘         │
└────────────────────────────────────┬───────────────────────────────────────────────────────────────────┘
                                     │
                                     ▼
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 4. MASTER OUTPUT STAGE                                                                                 │
├────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Combined Ocean & Acoustic Signal ──▶ oceanMasterGain (Scaled by volMaster)                             │
│                                             │                                                          │
│                                             ▼                                                          │
│                                  🔊 audioCtx.destination                                               │
│                                  (Hardware Audio Output)                                               │
└────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Algorithmic Buffer Generators

The engine does not load audio assets over HTTP. Instead, when `initAmbientAudio()` runs, it computes three high-density procedural memory buffers:

### 1. Leaky Integrated Brownian Noise (`createStereoBrownNoiseBuffer`)
* **Duration**: 10.0 seconds (Stereo, 2 channels).
* **Algorithm**: First-order leaky integrator on uniform white noise with channel-decoupled random state:
  $$\text{lastLeft} = \frac{\text{lastLeft} + 0.022 \cdot w_L[n]}{1.018}, \quad y_L[n] = 3.2 \cdot \text{lastLeft}$$
* **Acoustic Character**: Deep, organic, non-peaking acoustic body with $-6\text{ dB/octave}$ spectral roll-off.

### 2. High-Frequency White Noise Buffer (`createWhiteNoiseBuffer`)
* **Duration**: 5.0 seconds (Mono, loopable).
* **Algorithm**: Pure uniform random variable $w[n] \in [-1.0, 1.0]$.
* **Role**: Carrier source for shoreline impact transient bursts, resonant air cavity formants, and undertow suction.

### 3. Granular Micro-Bubble & Pebble Fizz (`createGranularFoamBuffer`)
* **Duration**: 6.0 seconds (Stereo).
* **Algorithm**: Non-linear Poisson pulse generator coupled with a fast leaky decay resonator:
  $$\text{popTrigger}_L = \begin{cases} (2 \cdot \text{rand}() - 1) \cdot 2.4 & \text{if } \text{rand}() < 0.0035 \\ 0 & \text{otherwise} \end{cases}$$
  $$\text{energy}_L[n] = (\text{energy}_L[n-1] \cdot 0.935) + \text{popTrigger}_L + (w_L[n] \cdot 0.12)$$
* **Acoustic Character**: Simulates millions of individual water micro-bubbles popping and fine wet sand/pebble friction without high-frequency harshness.

---

## 4. Acoustic Layer Breakdown & Physics Modeling

### Layer 1: The Distant Background Sea Bed
* **Nodes**: `bgSource` (Brown Noise) $\rightarrow$ `subCleanFilter` (HPF 75Hz) $\rightarrow$ `backgroundBodyFilter` (LPF 560Hz, Q=0.85) $\rightarrow$ `backgroundBodyGain` ($0.28 \times \text{volWaves}$).
* **Breathing LFO**: A sub-audible $1/12\text{ Hz}$ sine wave modulates the 560Hz cutoff frequency by $\pm 45\text{Hz}$ over a 12-second period.
* **Role**: Provides a warm, continuous, comforting oceanic foundation so foreground waves never emerge from dead digital silence.

---

### Layer 2: Discrete Foreground Shoreline Wave Sets
Individual foreground waves are triggered via `scheduleNextForegroundWave()` using a biological wave set progression:

```
Set Wave 1 (Starter, Intensity: 0.90) ──▶ Set Wave 2 (Rising, Intensity: 1.25) ──▶ Set Wave 3 (Apex King Wave, Intensity: 1.65) ──▶ Peaceful Lull (7s–12s)
```

Each wave event executes the following 6-stage physical pipeline:

#### A. The Swell Gathering (Attack Curve)
* **Duration**: $1.1\text{s} - 1.55\text{s}$.
* **Envelope**: 3-stage piecewise linear curve to model physical water mass gathering:
  - $0.0001 \rightarrow 0.35 \times \text{intensity}$ at $45\%$ duration.
  - $\rightarrow 0.85 \times \text{intensity}$ at $80\%$ duration.
  - $\rightarrow 1.65 \times \text{intensity}$ at peak crash time.
* **Frequency Sweep**: Lowpass cutoff sweeps dynamically from $220\text{Hz} \rightarrow 2400\text{–}2850\text{Hz}$.

#### B. The Barrel Formant Air Pocket ($420\text{Hz}$)
* **Timing**: Gathers $0.55\text{s}$ before the crest break.
* **Filter**: Biquad Bandpass at $420\text{Hz}$ with steep $Q=3.2$.
* **Role**: Simulates the hollow acoustic body of air trapped beneath a pitching wave lip before impact.

#### C. The Full-Spectrum Shoreline Crash Break
* **Timing**: Initiates at `crashPeakTime`.
* **Filter Cascade**: Wide-spectrum dual filter — **Highpass $380\text{Hz}$ ($Q=0.707$) + Lowpass $4800\text{Hz}$ ($Q=0.707$)**.
* **Envelope**: $40\text{ms}$ ultra-fast physical impact attack followed by a $2.5\text{s}$ natural exponential water wash decay.

#### D. Granular Foam Sizzle & Pebble Wash
* **Filter**: Highpass $2200\text{Hz}$ + Bandpass $4600\text{Hz}$ ($Q=0.8$).
* **Envelope**: $100\text{ms}$ rise, sustaining over $1.5\text{s}$, smoothly fading over the full decay window.

#### E. Shoreline Mass Impact Thud ($130\text{Hz}$)
* **Filter**: Bandpass $130\text{Hz}$ ($Q=2.4$).
* **Envelope**: $60\text{ms}$ punch attack with $1.6\text{s}$ low-frequency dissipation, simulating thousands of kilograms of water hitting the beach.

#### F. Prolonged Meditative Undertow Drag ("The Fluid Backwash")
* **Duration**: **$4.8\text{s}$ to $5.6\text{s}$** continuous meditative suction.
* **Acoustic Gravity Sweep**: Bandpass center frequency sweeps exponentially downwards from **$1950\text{Hz} \rightarrow 580\text{Hz}$** ($Q=1.6$).
* **Spatial Inward Pull**: Spatial panner pulls from the shoreline edge back inward toward center-field sea horizon ($0.0$).

---

### Layer 3: Dedicated Treble Softening Filter (`crashFoamTrebleFilter`)
* **Node**: Biquad Lowpass filter positioned directly after the crash and foam sub-buses.
* **Frequency Range**: $700\text{Hz}$ to $7000\text{Hz}$ ($20\% - 200\%$).
* **Default Setting**: **`45%`** ($1620\text{Hz}$, $Q=0.707$).
* **Role**: Completely removes harsh, intrusive top-end hiss/spray, ensuring the soundscape remains velvety, calm, and soothing.

---

### Layer 4: Harmonic Crystal Quartz Bowls (Just Intonation)
* **Acoustic Tuning**: Root $432\text{Hz}$, Just Major 3rd ($540\text{Hz}$, ratio 5:4), Just Perfect 5th ($648\text{Hz}$, ratio 3:2).
* **Pure Sines**: Independent ultra-slow sine LFOs create smooth, non-fatiguing sacred geometry breathing.
* **Default**: `0%` (available anytime in the settings drawer).

---

### Layer 5: Binaural Theta Entrainment & Sub-Bass
* **Binaural Channel**: Left Ear: $100\text{Hz}$, Right Ear: $106\text{Hz}$ ($\Delta f = 6.0\text{Hz}$ Theta frequency for deep meditative absorption).
* **Sub-Bass Drone**: Dual low-frequency oscillators at $55\text{Hz}$ and $110\text{Hz}$ providing deep, grounded warmth.

---

## 5. UI Control Map & Calibrated Meditative Defaults

All parameters are configured in the collapsible settings drawers and persisted to `localStorage` and cookies:

| Slider ID | Turkish Label | Param Key | Default | DSP Range | Web Audio Destination Node |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `#volumeSlider` | **Ses Düzeyi** | `vol_master` | **`14%`** | $0 - 100\%$ | `oceanMasterGain.gain` (scaled to max 0.65) |
| `#sliderWaves` | **🌊 Dalgalar (Gövde)** | `vol_waves` | **`115%`** | $0 - 200\%$ | `wavesMasterGain` & `backgroundBodyGain` ($0.28\times$) |
| `#sliderCrash` | **💥 Kırılma** | `vol_crash` | **`22%`** | $0 - 200\%$ | `crashMasterGain.gain` ($2.8\times$ multiplier) |
| `#sliderFoam` | **🫧 Köpük & Kum** | `vol_foam` | **`38%`** | $0 - 200\%$ | `foamMasterGain.gain` ($2.5\times$ multiplier) |
| `#sliderTrebleDamp`| **🕊️ Tiz Tıraşlama** | `vol_trebledamp`| **`45%`** | $20 - 200\%$ | `crashFoamTrebleFilter.frequency` ($700\text{–}7000\text{Hz}$) |
| `#sliderBarrel` | **🌀 Dalga Tüpü** | `vol_barrel` | **`28%`** | $0 - 200\%$ | `barrelMasterGain.gain` ($2.2\times$ multiplier) |
| `#sliderSwellCutoff`| **🌊 Kabarma Tavanı**| `vol_swell_cutoff`|**`70%`**| $20 - 300\%$ | Wave swell LPF peak cutoff multiplier ($480\text{–}7200\text{Hz}$) |
| `#sliderUndertow`| **💨 Geri Çekilme** | `vol_undertow` | **`45%`** | $0 - 250\%$ | `undertowMasterGain.gain` ($2.0\times$ multiplier) |
| `#sliderCadence` | **⏱️ Dalga Sıklığı** | `vol_cadence` | **`55%`** | $30 - 300\%$ | Reschedules wave set timer dynamically |
| `#sliderBowl1` | **🥣 Çanak I (432Hz)** | `vol_bowl1` | **`0%`** | $0 - 200\%$ | `bowlGain1.gain` ($0.16\times$) |
| `#sliderBowl2` | **🥣 Çanak II (540Hz)**| `vol_bowl2` | **`0%`** | $0 - 200\%$ | `bowlGain2.gain` ($0.14\times$) |
| `#sliderBowl3` | **🥣 Çanak III (648Hz)**| `vol_bowl3` | **`0%`** | $0 - 200\%$ | `bowlGain3.gain` ($0.12\times$) |
| `#sliderBinaural` | **🧠 Biyonoral** | `vol_binaural` | **`8%`** | $0 - 200\%$ | `binauralGainNode.gain` (independent channel) |
| `#sliderSubBass` | **🎵 Alt Bas** | `vol_subbass` | **`24%`** | $0 - 200\%$ | `droneGain.gain` (55Hz / 110Hz warmth) |

---

## 6. Build, Test & Deployment Workflow

Because the application is distributed as a single encrypted client-side package, **`app.js` is never edited directly**.

```bash
# 1. Edit the audio engine in build_app_js.js
# 2. Compile app.js
node build_app_js.js

# 3. Verify JS syntax
node -e "new Function(require('fs').readFileSync('app.js', 'utf8')); console.log('Syntax OK');"

# 4. Re-encrypt payload with AES-GCM (password: [passphrase]) and bust cache
node build.js

# 5. Verify decryption test suite
node test_appjs_decrypt.js

# 6. Commit and deploy
git add app.js index.html styles.css AUDIO_ENGINE_README.md
git commit -m "Your descriptive commit message"
git push origin main
```

---

## 7. Real-Time Telemetry & Console Diagnostics

To monitor the audio engine in real time:
1. Open the browser Developer Tools (`F12` $\rightarrow$ **Console**).
2. Enter the book password and unlock.
3. Observe live formatted telemetry:

```text
🌊 [AudioEngine] toggleAudio called! Current state: isAudioPlaying= false
🌊 [AudioEngine] Audio turned ON. Starting scheduler in 800ms...
🌊 [AudioEngine] Initial wave timer firing!
🌊 [AudioEngine] 💥 TRIGGERING FOREGROUND WAVE | Intensity: 0.90 | Peel: Left | Swell: 1.25s | Crash Peak: +1.25s | Total: 5.85s | AudioCtx Time: 1.82s
🌊 [AudioEngine] ⏱️ Next wave in set scheduled in 2.8s
🌊 [AudioEngine] Slider 🕊️ Tiz Tıraşlama: 45% Treble Cutoff: 1620Hz
🌊 [AudioEngine] 🏖️ Wave Set Lull initiated. Next set in 10.4s
```
