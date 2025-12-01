---
layout: default
title: Naphome RK3308B Proposal
---

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

### 5.1 REW (Room EQ Wizard) Overview

**What is REW?**

REW (Room EQ Wizard) is a professional-grade acoustic measurement and analysis software tool used throughout the audio industry for speaker design, room acoustics analysis, and audio system optimization. It provides precise, repeatable measurements that enable data-driven tuning decisions.

**Why REW for Naphome?**

For the RK3308B-based Naphome system, REW measurements are essential for:

1. **Objective Performance Validation:** Quantifying audio quality improvements over the ESP32-S3 v0.9 baseline
2. **DSP Tuning:** Providing frequency response data to guide equalization, beamforming, and AEC parameter optimization
3. **Manufacturing Readiness:** Establishing test procedures and pass/fail criteria for production units
4. **Investor/Factory Demonstrations:** Presenting measurable acoustic performance metrics

**REW Measurement Capabilities:**

REW generates comprehensive acoustic data including:
- **Frequency Response:** Full-spectrum (20Hz - 20kHz) amplitude vs. frequency
- **Impulse Response:** Time-domain characterization of speaker behavior
- **Phase Response:** Phase relationships across frequency spectrum
- **Distortion Analysis:** THD (Total Harmonic Distortion) and IMD (Intermodulation Distortion)
- **Waterfall Plots:** Frequency response over time (decay characteristics)
- **RT60 Measurements:** Room reverberation time (if applicable)

---

### 5.2 REW Measurement Protocol

**Test Setup:**

- **Calibrated Measurement Microphone:** Industry-standard reference mic (e.g., UMIK-1, Earthworks M30)
- **Controlled Environment:** Anechoic chamber or treated room with minimal reflections
- **Test Positions:** Multiple measurement points at 0.5m, 1m, 2m, and 3m from device
- **Angular Measurements:** 0°, 30°, 60°, 90° off-axis for beam pattern characterization
- **Volume Levels:** Measurements at 50%, 75%, and 100% of maximum output

**Measurement Sequence:**

1. **Baseline Measurements (Week 5):**
   - Frequency response sweeps (no DSP processing)
   - Impulse response capture
   - Distortion measurements at various volumes
   - Phase response analysis
   - Establishes "raw" hardware performance

2. **Beamforming Measurements (Week 6):**
   - Frequency response with beamforming active
   - Direction-of-arrival accuracy validation
   - Beam pattern characterization (polar plots)
   - Far-field performance at multiple distances
   - Multi-source rejection testing

3. **AEC Measurements (Week 7):**
   - Frequency response with AEC active
   - AEC convergence time measurements
   - Performance with Spotify playback at various volumes
   - Background noise rejection testing
   - Stability validation under dynamic conditions

4. **Final Tuning Measurements (Week 8):**
   - Complete system frequency response
   - Final distortion measurements
   - Production-ready tuning validation
   - Manufacturing test procedure validation

**Data Utilization:**

REW measurement data directly informs:

- **DSP Parameter Tuning:** Frequency response curves guide EQ filter design
- **Beamforming Optimization:** Polar plots validate beam pattern accuracy
- **AEC Algorithm Tuning:** Impulse response data optimizes echo cancellation
- **Production Test Procedures:** Baseline measurements establish pass/fail criteria
- **Documentation:** Measurement screenshots and data included in final acoustic report

---

### 5.3 Additional Acoustic Testing

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
- [ ] Coordinate with Vinaya (server) and Aman (app) on API specifications
- [ ] Design firmware-side server API integration architecture (REST/WebSocket)
- [ ] Implement device registration and authentication endpoints (firmware side)
- [ ] Create basic server communication module (ESP32-S3 → Server)
- [ ] Test initial server connectivity and authentication

**Deliverables:**
- Spotify Connect device appears in Spotify app
- Music playback functional
- DSP framework ready for algorithm loading
- Server communication infrastructure established (firmware side)
- API specifications coordinated with server/app teams

**Hours:** ~45h (40h audio/DSP + 5h server integration coordination)

---

#### Week 4: Wake Word Pipeline & Server/App Integration → **Demo 2**

**Objective:** Complete core architecture with wake word detection and server/app integration

**Tasks:**
- [ ] Integrate wake word detection engine (e.g., Porcupine, Picovoice)
- [ ] Implement wake word DSP pipeline (pre-processing, VAD)
- [ ] Create wake word event handling and state machine
- [ ] Develop full command routing: ESP32-S3 ↔ RK3308B ↔ Cloud
- [ ] Test network provisioning flow (ESP32-S3 → RK3308B)
- [ ] Implement system status reporting and health checks
- [ ] Create unified logging system across both processors
- [ ] Coordinate with Vinaya (server) on firmware-side API implementation:
  - Device status sync endpoints (online/offline, battery, sensors)
  - Command queue and execution handlers
  - Audio state sync (playback status, volume, track info)
  - Wake word event reporting
  - OTA update coordination
- [ ] Coordinate with Aman (app) on device communication protocol:
  - Mobile app ↔ Server ↔ Device command flow validation
  - Real-time status updates (WebSocket or polling)
  - Device discovery and pairing flow
  - Remote control endpoints (play/pause, volume, wake word trigger)
- [ ] Integrate firmware-side server API endpoints (Vinaya's server integration)
- [ ] Test end-to-end: App (Aman) → Server (Vinaya) → ESP32-S3 → RK3308B
- [ ] Validate error handling and reconnection logic
- [ ] Prepare demo script and documentation

**Deliverables:**
- Wake word detection functional
- Full dual-processor architecture operational
- Server API integration complete (firmware side, coordinated with Vinaya)
- App communication functional (coordinated with Aman)
- **Demo 2 ready for presentation** (includes server/app integration)

**Hours:** ~45h (30h wake word/architecture + 15h server/app integration coordination)

---

### Month 2 — January: Testing, Acoustics, Tuning, UX Integration

**Goal:** validate whole system, run full acoustic tests, produce manufacturing-ready tuning.

---

#### Week 5: Acoustic Test Setup & Baseline Measurements

**Objective:** Establish acoustic testing infrastructure and baseline performance

**Tasks:**
- [ ] Set up REW (Room EQ Wizard) measurement rig with calibrated reference microphone
- [ ] Configure controlled test environment (minimal reflections, consistent positioning)
- [ ] Create automated REW measurement scripts for repeatable testing
- [ ] Perform baseline frequency response sweeps (20Hz-20kHz, no DSP processing)
- [ ] Measure impulse response and phase characteristics using REW
- [ ] Test distortion levels (THD, IMD) at 50%, 75%, and 100% output volumes
- [ ] Capture waterfall plots for decay analysis
- [ ] Document baseline acoustic performance with REW screenshots and data exports
- [ ] Set up far-field test positions (0.5m, 1m, 2m, 3m markers) for future measurements
- [ ] Establish angular measurement positions (0°, 30°, 60°, 90° off-axis)
- [ ] Create measurement report template with REW data integration
- [ ] Compare baseline measurements against ESP32-S3 v0.9 performance

**Deliverables:**
- REW acoustic test rig fully operational
- Baseline measurements documented with REW data files
- Test procedures standardized and repeatable
- Baseline performance report with frequency response, distortion, and phase data

**Hours:** ~20h (acoustics) + ~20h (engineering)

---

#### Week 6: Beamforming & Far-Field Validation → **Demo 3**

**Objective:** Validate beamforming algorithms and far-field performance

**Tasks:**
- [ ] Implement beamforming algorithm (delay-and-sum or adaptive)
- [ ] Tune beamforming parameters for mic array geometry
- [ ] Test direction-of-arrival (DoA) accuracy
- [ ] Measure beam pattern characteristics using REW polar plot measurements
- [ ] Perform REW frequency response measurements with beamforming active at multiple angles
- [ ] Validate wake word detection at 0.5m, 1m, 2m, 3m distances
- [ ] Test multi-source rejection (background noise, competing speakers)
- [ ] Create DoA visualization tool
- [ ] Compare REW measurements: baseline vs. beamforming active
- [ ] Document beamforming performance metrics with REW data
- [ ] Generate REW measurement screenshots for demo presentation
- [ ] Prepare demo materials (recordings, visualizations, REW measurement reports)
- [ ] Initial factory liaison coordination:
  - Review hardware specifications with factory/OEM partners
  - Provide firmware requirements documentation
  - Coordinate on DVT (Design Verification Test) planning

**Deliverables:**
- Beamforming functional and tuned
- Far-field wake word performance validated
- REW measurement data showing beamforming improvements
- Factory coordination initiated
- **Demo 3 ready for presentation** (includes REW measurement demonstrations)

**Hours:** ~20h (acoustics) + ~22h (engineering: 20h beamforming + 2h factory liaison)

---

#### Week 7: AEC Integration, Environmental Testing & Server/App Validation

**Objective:** Enable AEC, validate performance, and complete server/app integration testing

**Tasks:**
- [ ] Integrate AEC (Acoustic Echo Cancellation) algorithm
- [ ] Test AEC convergence with Spotify playback
- [ ] Perform REW measurements with AEC active (frequency response, impulse response)
- [ ] Validate AEC performance at various volume levels using REW distortion measurements
- [ ] Test wake word detection with music playing
- [ ] Measure AEC performance in different room environments
- [ ] Test with various background noise conditions
- [ ] Validate AEC stability and convergence time using REW time-domain analysis
- [ ] Test edge cases (sudden volume changes, music pauses)
- [ ] Compare REW measurements: baseline vs. AEC active vs. AEC + Spotify
- [ ] Document AEC tuning parameters
- [ ] Create AEC performance report with REW measurement data
- [ ] Coordinate with Vinaya (server) and Aman (app) on integration testing:
  - Multi-device scenarios
  - Concurrent user sessions
  - Server failover and reconnection
  - Data synchronization accuracy
- [ ] Firmware-side integration testing:
  - All firmware endpoints responding correctly
  - Error handling and reconnection logic
  - Performance under various network conditions
- [ ] End-to-end integration testing: App (Aman) ↔ Server (Vinaya) ↔ Firmware
- [ ] Document firmware-side API specifications for server/app teams
- [ ] Factory liaison coordination:
  - Develop factory test procedures and specifications
  - Coordinate on production line test requirements
  - Provide firmware calibration procedures for factory
  - Review DVT hardware requirements with factory

**Deliverables:**
- AEC functional with Spotify playback
- Wake word works reliably with music on
- REW measurement data validating AEC performance
- Environmental testing complete
- Server integration fully validated (coordinated with Vinaya)
- App integration fully validated (coordinated with Aman)
- Factory test procedures documented

**Hours:** ~20h (acoustics) + ~28h (engineering: 20h AEC + 5h server/app + 3h factory liaison)

---

#### Week 8: Final Integration, Tuning & Production Readiness → **Demo 4**

**Objective:** Complete system integration, final tuning, and production preparation

**Tasks:**
- [ ] Integrate local STT (Speech-to-Text) pipeline
- [ ] Test complete voice flow: wake word → STT → LLM → TTS
- [ ] Validate sleep sounds and alarm functionality
- [ ] Test all sensor integrations (temp, humidity, LEDs)
- [ ] Perform regression testing across all features
- [ ] Final acoustic tuning using REW measurement data to optimize DSP parameters
- [ ] Perform final REW measurements: complete system frequency response, distortion, phase
- [ ] Validate production-ready tuning against REW measurement targets
- [ ] Create production-ready tuning library
- [ ] Document calibration procedures using REW measurement protocols
- [ ] Prepare manufacturing test requirements based on REW baseline measurements
- [ ] Create final acoustic tuning report with comprehensive REW measurement data
- [ ] Coordinate final server/app integration polish with Vinaya and Aman:
  - Performance optimization validation
  - Security audit coordination (authentication, encryption)
  - Error recovery and edge case handling testing
  - Load testing and scalability validation
- [ ] Complete end-to-end system testing: App (Aman) → Server (Vinaya) → Firmware → Hardware
- [ ] Prepare comprehensive demo showcasing all features including REW measurement demonstrations and server/app integration
- [ ] Document firmware system architecture and API specifications (for server/app teams)
- [ ] Final factory liaison and production readiness:
  - Complete factory test procedure documentation
  - Production line test setup specifications
  - Firmware release package for factory (binaries, calibration data, test scripts)
  - Production readiness review with factory/OEM partners
  - DVT hardware validation coordination
  - Factory training materials (if required)
  - Production sign-off documentation

**Deliverables:**
- All features integrated and tested
- Production-ready tuning parameters validated with REW measurements
- Final acoustic report with complete REW measurement data and analysis
- Manufacturing test procedures based on REW measurement protocols
- Factory test procedures and production line specifications complete
- Firmware release package ready for factory
- Server/app integration production-ready (coordinated with Vinaya and Aman)
- **Demo 4 ready for presentation** (includes final REW measurement demonstrations and full server/app integration)
- Complete documentation package (firmware APIs, integration specs, factory documentation)

**Hours:** ~20h (acoustics) + ~30h (engineering: 20h tuning/testing + 5h server/app + 5h factory liaison)

---

## 7. Billing & Cost Summary

### Team Coordination Note

**Server and App Development:** Server development (by Vinaya) and app development (by Aman) will proceed in parallel with firmware development. This proposal covers firmware-side integration work, API coordination, and end-to-end testing. Firmware tasks focus on integration points and coordination rather than building server/app components.

---

### Weekly Hours Breakdown

| Week | Engineering Hours | Acoustics Hours | Total Hours | Demo |
|------|------------------|-----------------|-------------|------|
| Week 1 | 40h | — | 40h | — |
| Week 2 | 40h | — | 40h | **Demo 1** |
| Week 3 | 45h | — | 45h | — |
| Week 4 | 45h | — | 45h | **Demo 2** |
| Week 5 | 20h | 20h | 40h | — |
| Week 6 | 22h | 20h | 42h | **Demo 3** |
| Week 7 | 28h | 20h | 48h | — |
| Week 8 | 30h | 20h | 50h | **Demo 4** |
| **Total** | **270h** | **60h** | **330h** | **4 Demos** |

*Note: Factory liaison hours included in Weeks 6-8 (10h total) for coordination with factory/OEM partners.*

### December Costs (Weeks 1-4)
- 170 engineering hours × $100/hr = **$17,000**
- After-hours (40h × $150/hr) = **$6,000**

**Total December: $23,000**

### January Costs (Weeks 5-8)
- 100 engineering hours × $100/hr = **$10,000**
- 60 acoustics hours × $100/hr = **$6,000**
- After-hours (40h × $150/hr) = **$6,000**

**Total January: $22,000**

### Total 2-Month Project Cost

**$45,000**

*Note: Hours are estimates and may vary based on hardware availability and integration complexity. After-hours work is scheduled for critical path items and demo preparation. Server/app development by Vinaya and Aman proceeds in parallel and is not included in this firmware development proposal.*

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
- Server API integration complete (firmware side, coordinated with Vinaya)
- App communication functional (coordinated with Aman)
- **Demo 2: Spotify + Wake Word Architecture** (includes server/app integration)

### Week 6 Deliverables (Demo 3)
- REW acoustic test infrastructure established and operational
- Baseline REW measurements documented (frequency response, distortion, phase)
- Beamforming algorithm implemented and tuned
- REW measurements with beamforming active (polar plots, frequency response at multiple angles)
- Far-field wake word performance validated (0.5m - 3m)
- Direction-of-arrival accuracy confirmed
- REW measurement data comparing baseline vs. beamforming performance
- Factory coordination initiated (hardware specs review, DVT planning)
- **Demo 3: Beamforming + Far-Field Capture** (includes REW measurement demonstrations)

### Week 8 Deliverables (Demo 4)
- AEC functional with Spotify playback
- REW measurements validating AEC performance
- Local STT pipeline integrated
- Complete voice flow operational (wake word → STT → LLM → TTS)
- All features integrated (sleep sounds, alarms, sensors, LEDs)
- Production-ready tuning parameters validated with REW measurements
- Final acoustic tuning report with comprehensive REW measurement data
- Manufacturing test procedures documented (based on REW measurement protocols)
- Factory test procedures and production line specifications complete
- Firmware release package ready for factory (binaries, calibration data, test scripts)
- Server/app integration production-ready (coordinated with Vinaya and Aman)
- Complete firmware system documentation
- **Demo 4: Production-Ready Acoustic + Voice Demo** (includes final REW measurement demonstrations and full server/app integration)

### Final Deliverables (End of Week 8)
- Fully functional RK3308B + ESP32-S3 dual-processor system
- Production-ready firmware and tuning libraries
- Complete acoustic evaluation report with full REW measurement data:
  - Baseline measurements (frequency response, distortion, phase, impulse response)
  - Beamforming measurements (polar plots, angular frequency response)
  - AEC measurements (performance with Spotify, convergence analysis)
  - Final system measurements (complete frequency response, distortion, phase)
- REW measurement data files and screenshots for all test phases
- Firmware system architecture documentation
- Firmware API specifications (for server/app teams - Vinaya and Aman)
- Manufacturing test requirements (based on REW baseline measurements)
- Factory test procedures and production line specifications
- Firmware release package for factory (binaries, calibration data, test scripts)
- Production readiness documentation for factory/OEM partners
- All 4 demonstrations completed and documented
- End-to-end integration validated: App (Aman) ↔ Server (Vinaya) ↔ Firmware

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
