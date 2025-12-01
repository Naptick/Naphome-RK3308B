# Naphome Firmware Development Proposal

## Transition from ESP32-S3 v0.9 → RK3308B Architecture

**Spotify SDK · Local STT · Beamforming Mic Array · Speaker Output · 60h Acoustics**

**Two-Month Development Plan: December Architecture → January Testing & Demonstrations**

---

## 1. Objective

This proposal outlines the transition from the existing Naphome v0.9 ESP32-S3–only firmware architecture to a dual-processor RK3308B + ESP32-S3 design, enabling:

- Higher audio quality
- Local wake-word + local STT
- True beamforming
- High-fidelity speaker output
- Spotify SDK / Spotify Connect integration
- Vastly improved AEC
- Mass-production readiness

The two-month plan includes showcase-ready demonstrations at strategic milestones (end-of-week demos).

---

## 2. Background: Transition Path

### 2.1 Current Architecture: v0.9 ESP32-S3

The v0.9 system (your current hardware) includes:

- Basic audio playback
- BLE provisioning
- LED control
- Temp/humidity sensors
- Cloud routines
- Light-duty LLM bridges
- Limited audio processing
- No beamforming
- No local STT
- No Spotify Connect natively

**Limitation:** the ESP32-S3 alone cannot provide far-field audio quality comparable to modern smart speakers.

---

### 2.2 Intermediate Prototype (Existing Pi Version)

Used for validating:

- mic/speaker loop
- Spotify (spotifyd)
- cloud STT → LLM → TTS
- LED + sensors
- IoT backend
- Voice-to-Voice

This prototype validates the function, but not the architecture.

---

### 2.3 Final Target Architecture: RK3308B + ESP32-S3

This platform supports:

- Full DSP pipeline
- Beamforming mic array
- Local wake word
- Local STT
- High-end speaker output
- Stable Spotify Connect endpoint
- Mass manufacturability
- Fast boot + low power

---

## 3. Demonstration Milestones

Below are concrete demo deliverables at the end of each major sprint.
These are planned intentionally so the factory, investors, or your internal leads can see steady progress.

---

### ✅ Demo 1 — December Week 2

**"Hello RK3308B" Audio Bring-Up Demo**

**What we show:**
- RK3308B boots Linux
- Audio output → amplifier (test tone)
- Mic array capturing raw channels
- ESP32-S3 connected + sending commands
- LEDs responding to RK commands

**Purpose:**
Proves the new silicon stack is alive, integrated, and controllable.

---

### ✅ Demo 2 — December Week 4

**Spotify + Wake Word Architecture Demo**

**What we show:**
- Spotify Connect playback
- Wake word DSP initialization
- ESP32-S3 provisioning → RK network online
- Command routing between processors
- Live debug over USB-CDC

**Purpose:**
Shows the full architecture works end-to-end.

---

### ✅ Demo 3 — January Week 2

**Beamforming + Far-Field Capture Demo**

**What we show:**
- Beamformed audio samples at 0.5m, 1m, 2m, 3m
- Direction-of-arrival visualization
- Wake word triggering at real distances
- REW measurement screenshots

**Purpose:**
Confirms core acoustic capabilities for the factory.

---

### ✅ Demo 4 — January Week 4

**Production-Ready Acoustic + Voice Demo**

**What we show:**
- AEC functional while Spotify is playing
- Wake word works with music on
- Local STT confirmation
- Sleep sounds, alarms, LEDs, sensors all integrated
- Final acoustic tuning report

**Purpose:**
This is the "confidence demo" — showing readiness for DVT hardware.

---

## 4. System Architecture Overview

*(Architecture details to be included - placeholder for completeness)*

The RK3308B + ESP32-S3 dual-processor architecture provides:

- **RK3308B (Main Audio Processor):**
  - Linux-based firmware
  - Audio DSP pipeline
  - Beamforming algorithms
  - Wake word detection
  - Local STT processing
  - Spotify Connect integration
  - High-quality audio I/O

- **ESP32-S3 (IoT & Control):**
  - BLE provisioning
  - Cloud connectivity
  - Sensor management
  - LED control
  - Inter-processor communication bridge
  - Low-power standby modes

- **Communication:**
  - UART/SPI between processors
  - Command/response protocol
  - Audio metadata exchange
  - Status synchronization

---

## 5. Acoustic Evaluation (60 Hours)

The acoustic evaluation phase includes comprehensive testing and tuning:

- **REW (Room EQ Wizard) Measurements:**
  - Frequency response sweeps
  - Impulse response analysis
  - Distortion measurements
  - Phase alignment

- **Beamforming Validation:**
  - Direction-of-arrival accuracy
  - Beam pattern characterization
  - Far-field performance (0.5m - 3m)
  - Multi-source rejection

- **AEC (Acoustic Echo Cancellation) Testing:**
  - Performance with music playback
  - Background noise handling
  - Convergence time
  - Stability under various conditions

- **Wake Word Performance:**
  - Detection range
  - False positive rate
  - Performance with music
  - Multi-user scenarios

- **Final Tuning:**
  - Production-ready DSP parameters
  - Calibration procedures
  - Manufacturing test requirements

---

## 6. Two-Month Development Plan with Weekly Tasks & Biweekly Demonstrations

### Month 1 — December: RK3308B Architecture Bring-Up

**Goal:** move from ESP32-S3-only logic → full RK3308B firmware architecture  
**Key focus:** enabling Spotify, audio, mic array, wake-word DSP, and inter-processor communication.

---

#### Week 1: Foundation & Hardware Integration

**Objective:** Establish RK3308B Linux environment and basic inter-processor communication

**Tasks:**
- [ ] Set up RK3308B development environment (Buildroot/Yocto)
- [ ] Configure Linux kernel for audio subsystems (ALSA, I²S, TDM)
- [ ] Implement UART/SPI communication protocol between RK3308B ↔ ESP32-S3
- [ ] Create command/response message format specification
- [ ] Develop basic ESP32-S3 firmware updates for dual-processor mode
- [ ] Test boot sequence and power management
- [ ] Establish USB-CDC debug interface
- [ ] Create initial device tree overlays for audio hardware

**Deliverables:**
- RK3308B boots to Linux shell
- ESP32-S3 can send/receive commands to/from RK3308B
- Basic logging and debug infrastructure operational

**Hours:** ~40h

---

#### Week 2: Audio I/O & Basic Control → **Demo 1**

**Objective:** Enable audio input/output and demonstrate basic system integration

**Tasks:**
- [ ] Configure I²S output driver for speaker amplifier
- [ ] Implement TDM interface for 4-channel mic array capture
- [ ] Create ALSA configuration for playback/record
- [ ] Develop test tone generator for audio output validation
- [ ] Implement LED control commands from RK3308B
- [ ] Create sensor read commands (temp/humidity via ESP32-S3)
- [ ] Build basic command routing system
- [ ] Test audio latency and buffer management
- [ ] Validate mic array channel mapping and gain staging

**Deliverables:**
- Audio output functional (test tones)
- Mic array capturing all 4 channels
- LEDs controllable from RK3308B
- **Demo 1 ready for presentation**

**Hours:** ~40h

---

#### Week 3: Spotify Integration & DSP Framework

**Objective:** Enable Spotify Connect playback and prepare DSP pipeline

**Tasks:**
- [ ] Integrate librespot or Spotify Connect SDK
- [ ] Configure network stack (WiFi via ESP32-S3 or direct RK3308B)
- [ ] Implement audio routing: Spotify → I²S output
- [ ] Set up DSP framework (loadable modules, parameter control)
- [ ] Create DSP block initialization system
- [ ] Implement basic audio mixing (Spotify + system sounds)
- [ ] Test Spotify authentication and device discovery
- [ ] Validate audio quality and sample rate handling
- [ ] Create Spotify control API (play/pause/volume via commands)

**Deliverables:**
- Spotify Connect device appears in Spotify app
- Music playback functional
- DSP framework ready for algorithm loading

**Hours:** ~40h

---

#### Week 4: Wake Word Pipeline & Full Architecture → **Demo 2**

**Objective:** Complete core architecture with wake word detection

**Tasks:**
- [ ] Integrate wake word detection engine (e.g., Porcupine, Picovoice)
- [ ] Implement wake word DSP pipeline (pre-processing, VAD)
- [ ] Create wake word event handling and state machine
- [ ] Develop full command routing: ESP32-S3 ↔ RK3308B ↔ Cloud
- [ ] Test network provisioning flow (ESP32-S3 → RK3308B)
- [ ] Implement system status reporting and health checks
- [ ] Create unified logging system across both processors
- [ ] Validate end-to-end command flow
- [ ] Prepare demo script and documentation

**Deliverables:**
- Wake word detection functional
- Full dual-processor architecture operational
- **Demo 2 ready for presentation**

**Hours:** ~40h

---

### Month 2 — January: Testing, Acoustics, Tuning, UX Integration

**Goal:** validate whole system, run full acoustic tests, produce manufacturing-ready tuning.

---

#### Week 5: Acoustic Test Setup & Baseline Measurements

**Objective:** Establish acoustic testing infrastructure and baseline performance

**Tasks:**
- [ ] Set up REW (Room EQ Wizard) measurement rig
- [ ] Configure calibrated microphone and test environment
- [ ] Create automated measurement scripts
- [ ] Perform baseline frequency response sweeps (no processing)
- [ ] Measure impulse response and phase characteristics
- [ ] Test distortion levels at various output volumes
- [ ] Document baseline acoustic performance
- [ ] Set up far-field test positions (0.5m, 1m, 2m, 3m markers)
- [ ] Create measurement report template

**Deliverables:**
- Acoustic test rig operational
- Baseline measurements documented
- Test procedures standardized

**Hours:** ~20h (acoustics) + ~20h (engineering)

---

#### Week 6: Beamforming & Far-Field Validation → **Demo 3**

**Objective:** Validate beamforming algorithms and far-field performance

**Tasks:**
- [ ] Implement beamforming algorithm (delay-and-sum or adaptive)
- [ ] Tune beamforming parameters for mic array geometry
- [ ] Test direction-of-arrival (DoA) accuracy
- [ ] Measure beam pattern characteristics
- [ ] Validate wake word detection at 0.5m, 1m, 2m, 3m distances
- [ ] Test multi-source rejection (background noise, competing speakers)
- [ ] Create DoA visualization tool
- [ ] Perform REW measurements with beamforming active
- [ ] Document beamforming performance metrics
- [ ] Prepare demo materials (recordings, visualizations, measurements)

**Deliverables:**
- Beamforming functional and tuned
- Far-field wake word performance validated
- **Demo 3 ready for presentation**

**Hours:** ~20h (acoustics) + ~20h (engineering)

---

#### Week 7: AEC Integration & Environmental Testing

**Objective:** Enable AEC and validate performance under real-world conditions

**Tasks:**
- [ ] Integrate AEC (Acoustic Echo Cancellation) algorithm
- [ ] Test AEC convergence with Spotify playback
- [ ] Validate AEC performance at various volume levels
- [ ] Test wake word detection with music playing
- [ ] Measure AEC performance in different room environments
- [ ] Test with various background noise conditions
- [ ] Validate AEC stability and convergence time
- [ ] Test edge cases (sudden volume changes, music pauses)
- [ ] Document AEC tuning parameters
- [ ] Create AEC performance report

**Deliverables:**
- AEC functional with Spotify playback
- Wake word works reliably with music on
- Environmental testing complete

**Hours:** ~20h (acoustics) + ~20h (engineering)

---

#### Week 8: Final Integration, Tuning & Production Readiness → **Demo 4**

**Objective:** Complete system integration, final tuning, and production preparation

**Tasks:**
- [ ] Integrate local STT (Speech-to-Text) pipeline
- [ ] Test complete voice flow: wake word → STT → LLM → TTS
- [ ] Validate sleep sounds and alarm functionality
- [ ] Test all sensor integrations (temp, humidity, LEDs)
- [ ] Perform regression testing across all features
- [ ] Final acoustic tuning and DSP parameter optimization
- [ ] Create production-ready tuning library
- [ ] Document calibration procedures
- [ ] Prepare manufacturing test requirements
- [ ] Create final acoustic tuning report
- [ ] Prepare comprehensive demo showcasing all features
- [ ] Document system architecture and API specifications

**Deliverables:**
- All features integrated and tested
- Production-ready tuning parameters
- Final acoustic report
- **Demo 4 ready for presentation**
- Complete documentation package

**Hours:** ~20h (acoustics) + ~20h (engineering)

---

## 7. Billing & Cost Summary

### Weekly Hours Breakdown

| Week | Engineering Hours | Acoustics Hours | Total Hours | Demo |
|------|------------------|-----------------|-------------|------|
| Week 1 | 40h | — | 40h | — |
| Week 2 | 40h | — | 40h | **Demo 1** |
| Week 3 | 40h | — | 40h | — |
| Week 4 | 40h | — | 40h | **Demo 2** |
| Week 5 | 20h | 20h | 40h | — |
| Week 6 | 20h | 20h | 40h | **Demo 3** |
| Week 7 | 20h | 20h | 40h | — |
| Week 8 | 20h | 20h | 40h | **Demo 4** |
| **Total** | **240h** | **60h** | **300h** | **4 Demos** |

### December Costs (Weeks 1-4)
- 160 engineering hours × $100/hr = **$16,000**
- After-hours (40h × $150/hr) = **$6,000**

**Total December: $22,000**

### January Costs (Weeks 5-8)
- 60 engineering hours × $100/hr = **$6,000**
- 60 acoustics hours × $100/hr = **$6,000**
- After-hours (40h × $150/hr) = **$6,000**

**Total January: $20,000**

### Total 2-Month Project Cost

**$42,000**

*Note: Hours are estimates and may vary based on hardware availability and integration complexity. After-hours work is scheduled for critical path items and demo preparation.*

---

## 8. Deliverable Summary

### Week 2 Deliverables (Demo 1)
- RK3308B Linux environment operational
- Audio I/O functional (test tones, mic array capture)
- ESP32-S3 ↔ RK3308B communication established
- LED and sensor control integrated
- **Demo 1: "Hello RK3308B" Audio Bring-Up**

### Week 4 Deliverables (Demo 2)
- Spotify Connect playback functional
- DSP framework initialized
- Wake word detection pipeline operational
- Full dual-processor architecture validated
- Network provisioning flow complete
- **Demo 2: Spotify + Wake Word Architecture**

### Week 6 Deliverables (Demo 3)
- Acoustic test infrastructure established
- Baseline measurements documented
- Beamforming algorithm implemented and tuned
- Far-field wake word performance validated (0.5m - 3m)
- Direction-of-arrival accuracy confirmed
- **Demo 3: Beamforming + Far-Field Capture**

### Week 8 Deliverables (Demo 4)
- AEC functional with Spotify playback
- Local STT pipeline integrated
- Complete voice flow operational (wake word → STT → LLM → TTS)
- All features integrated (sleep sounds, alarms, sensors, LEDs)
- Production-ready tuning parameters
- Final acoustic tuning report
- Manufacturing test procedures documented
- Complete system documentation
- **Demo 4: Production-Ready Acoustic + Voice Demo**

### Final Deliverables (End of Week 8)
- Fully functional RK3308B + ESP32-S3 dual-processor system
- Production-ready firmware and tuning libraries
- Complete acoustic evaluation report
- System architecture documentation
- API specifications
- Manufacturing test requirements
- All 4 demonstrations completed and documented

---

## 9. Conclusion

This proposal transitions Naphome from the v0.9 ESP32-S3 single-processor architecture to a mass-production-ready RK3308B + ESP32-S3 dual-processor platform, accomplishing the full stack of:

- audio
- DSP
- Spotify
- wake word
- STT
- acoustics
- cloud
- OTA
- UX demonstration milestones

This two-month schedule shows measurable progress every two weeks via clearly defined demonstration checkpoints, ensuring transparency, alignment, and predictable delivery.

---

## Next Steps

If you'd like, I can:

- 📘 turn this into a PDF
- 📊 create a Gantt-style graphic for milestones
- 📝 prepare a version specifically for factory/OEM partners
- 💰 prepare one formatted for investors

Just tell me which one.
