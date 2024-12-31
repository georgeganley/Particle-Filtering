## Acknowledgments

This project was completed as part of the AI for Robotics course at Georgia Tech. This repository does not include the code or solution to the project, but does illustrate the outcomes of a successful particle filtering implementation.

---

# Summary and Goal

The primary goal is to localize a satellite in its home solar system using a **particle filter** and specialized sensor measurements. The satellite emerges from a wormhole and is placed in an unknown orbit around the sun, and we must estimate its position accurately before resources run out.

---

## Overview of the Problem

### 1. Context
- A satellite is warped into its home solar system at an unknown location.  
- The solar system consists of a sun and multiple planets orbiting counter-clockwise.  
- The satellite itself moves in a circular, counter-clockwise orbit around the sun.

### 2. Sensors & Measurements
- **Part A:** A noisy *gravitational pull measurement* that represents the sum of the gravitational accelerations from all planets (excluding the sun).  
- **Part B:** Multiple *percent illumination measurements*, each indicating how “illuminated” a planet appears based on the phase angle among sun, planet, and satellite.

### 3. Localization Goal
- Determine the satellite’s \((x, y)\) position accurately (within **0.01 AU**).  
- The satellite could initially be up to **±4 AU** from the sun, so the particle filter must be robust in its initial coverage.  
- The solution must converge quickly (within 300 simulated days or before a **15-second CPU limit**).

### 4. Algorithmic Approach: Particle Filter
- **Initialization:** Distribute particles across plausible locations.  
- **Motion Model:** Update particle positions based on a bicycle (or orbital) motion model.  
- **Measurement Update (Importance Weights):** Adjust particle weights based on how likely each particle’s predicted measurement matches the real sensor reading.  
- **Resampling & Fuzzing:** Resample particles according to their weights, and introduce some randomness to avoid degeneracy.  
- **Estimation:** Combine the best particles (e.g., via weighted average) to estimate the satellite’s position.

### 5. Control & Communication
- Once localized with illumination data, we must send an *SOS message* to our home planet (the outermost planet in the system).  
- Return:
  1. The predicted location of the satellite.  
  2. The absolute angle to the home planet (in radians) for message transmission.  

---

## Requirements & Constraints

- **Sensor Readings:**  
  - *Part A:* Single gravitational magnitude measurement per day.  
  - *Part B:* Multiple percent illumination readings (one per planet, ordered by distance from the sun).  
- **Resource Limit:** You have up to **300 simulated days** before running out of supplies (or risk hitting the CPU timeout).  
- **Messaging:** Must transmit 10 SOS messages to the home planet for retrieval.


# Localization Results

Shown below are some examples of a successfully implemented particle filter for localizing the satellite. The particles in each example scenario are initiated in a uniform distribution across the x, y plane, then reweighted with each iteration according to which particles most accurately describe the location of the satellite.

![Particle Filter Localization](https://github.com/user-attachments/assets/a32bb152-f70a-4e45-b5d4-377d4e1a911c)
