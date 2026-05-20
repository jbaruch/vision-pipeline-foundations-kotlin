# Optimizing a Real-Time Face Recognition Pipeline for Live Events

## Problem/Feature Description

A company runs live-event check-in booths where a kiosk camera feeds into a Kotlin/JVM pipeline that matches attendees against a pre-registered face database. The current prototype runs face detection, face recognition, and emotion classification on every captured frame. On the demo hardware the CPU hits 95% utilization and the preview stream stutters noticeably, making the booth look unprofessional.

An engineer has profiled the pipeline and confirmed that the bottleneck is running all three inference stages at 30 frames per second when none of them need to operate that fast: face boxes in the preview barely move between consecutive frames, and emotions shift on a timescale of seconds, not milliseconds. The team needs the inference to run at a reduced cadence while the preview and camera capture continue to appear completely smooth. The solution must also address a separate quality issue: the Haar cascade face detector is producing a large number of false-positive rectangles on background textures and objects, and the team wants this cleaned up as part of the optimization work.

Write a Kotlin source file named `pipeline.kt` that implements the main capture-and-inference loop and any helper functions needed. Assume a `grabber` (`OpenCVFrameGrabber`) and `cascade` (`CascadeClassifier`) are already initialized. Include stub implementations (or clearly annotated TODOs) for any functions that depend on an external model (e.g. `recognizeFace()`, `classifyEmotion()`). Include a diagnostic function or log statement that reports the measured face-detection rate in Hz.

## Output Specification

Produce a Kotlin source file `pipeline.kt` containing:

- The main processing loop with appropriately gated inference calls
- The face detection helper that handles the resolution correctly
- Logic ensuring face boxes remain visible on-screen continuously even when detection is not running on every frame
- A `main()` function or clearly labelled entry point that runs the loop
- Diagnostic logging of face-detection frequency in Hz
