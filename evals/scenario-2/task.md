# Reactive Vision Pipeline Using Kotlin Coroutines

## Problem/Feature Description

A startup is building a visitor analytics system for retail stores. The backend is written entirely in Kotlin and uses coroutines throughout. The team's first attempt at a vision pipeline used a raw `while(true)` loop with manual counter variables for throttling, and the code quickly became hard to reason about: the detection cadence logic was tangled with the preview encoding, and adding a second inference stage (emotion classification) required re-auditing the entire loop to avoid off-by-one frame counter bugs.

The lead engineer wants to refactor the pipeline to use Kotlin `Flow` — separating the frame-producing stream from the two downstream inference streams (face detection and emotion classification), each running at an independently controlled rate. This idiomatic approach will make it obvious what each stage does, allow the two inference stages to be tested and replaced independently, and let the team use standard Flow operators for rate control rather than manual modulo arithmetic. The preview should still appear smooth to the end-user watching the live feed.

Face detection in this pipeline uses a Haar cascade running on the standard OpenCV stack; the team has already confirmed that Haar produces many false-positive rectangles at native camera resolution, so the detection stage needs to address this. Face recognition produces embeddings that require full detail.

Write a Kotlin source file named `reactive_pipeline.kt` that implements the Flows-based pipeline. Assume a `grabber` (`OpenCVFrameGrabber`) and `cascade` (`CascadeClassifier`) are already initialized. Use stub functions for model inference where necessary.

## Output Specification

Produce `reactive_pipeline.kt` containing:

- A `Flow<Mat>` producer that captures frames from the grabber
- A face-detection flow derived from the frame flow, operating at an appropriate reduced rate
- An emotion classification flow derived from the frame flow, operating at an appropriately lower rate than face detection
- The face detection implementation that handles frame resolution appropriately
- A `main()` or `runPipeline()` coroutine entry point that collects from all streams and updates a preview overlay with the latest known face boxes
