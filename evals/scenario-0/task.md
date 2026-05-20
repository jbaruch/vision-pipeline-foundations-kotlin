# Webcam Discovery Utility for macOS Development Workstation

## Problem/Feature Description

A distributed team has been demonstrating a real-time face recognition prototype on macOS laptops. During back-to-back conference calls the presenters kept Zoom, Teams, and OBS open in the background. When they launched the Kotlin/JavaCV pipeline, it would sometimes capture perfectly and other times produce only black frames or motion from the wrong camera — and the team never knew until the demo was already broken. The root cause turned out to be that virtual cameras installed by those apps were taking low device indices, pushing the physical webcam to a higher slot that changed each time.

The team wants a standalone Kotlin utility — a file called `camera_probe.kt` — that reliably locates the real physical webcam and returns its device index. The utility must handle the case where the grabber opens successfully but the camera delivers black frames (a common macOS condition), and must also allow a developer to force a specific camera index via an environment variable when they know their setup. The utility should also expose metadata about every probed camera (index, resolution, brightness) so engineers can inspect what was found.

## Output Specification

Produce a single Kotlin source file named `camera_probe.kt` that contains:

- A data class or equivalent structure capturing per-camera probe results (index, resolution, and measured brightness)
- A function that probes available cameras and returns a list of those results
- A function that selects the best real camera from that list
- A top-level Camera class (or equivalent entry point) that uses the selection logic on construction and implements the standard Java resource-management interface
- A short `main()` function that runs the probe and prints the chosen camera index plus the full list of probe results

Do not produce a `build.gradle` or any other build file — just the `.kt` source.
