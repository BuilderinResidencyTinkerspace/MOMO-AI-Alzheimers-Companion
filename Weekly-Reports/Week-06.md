WAKE WORD & SPEECH-TO-TEXT PIPELINE

Phase:
Voice Input Layer Development

Objective: 
Build and tune the voice input pipeline — wake word detection followed by accurate speech transcription.

Activities:

· Implemented Vosk wake-word detection with a grammar-restricted vocabulary (["momo", "hey momo", ...]) so the recognizer structurally cannot hallucinate non-wake phrases.

· Added a hallucination blocklist to quiet wake-word logs.

· Implemented audio preprocessing:

  · Capture at 44100 Hz (mic native), downsample to 16000 Hz for Vosk and Whisper.

  · Speech-band bandpass (300–3400 Hz) for RMS-based voice activity detection.

  · 50 Hz + 100 Hz notch filters to eliminate mains hum (FFT showed 50 Hz peak at ~10× the next component).

· Calibrated silence detection:

  · Measured silence ≈ 222 RMS, speech ≈ 1550 RMS → ratio ≈ 7×.

  · Set SILENCE_RMS_THRESHOLD = 300.

· Integrated faster-whisper small.en (upgraded from base.en) for improved accuracy on elderly speech.

· Set SILENCE_TIMEOUT = 1.2 s (tunable to 1.8 s if elderly speech is cut off mid-sentence).

Critical findings:

· Wake word detection worked reasonably well. Vosk's grammar restriction meant "Momo" and "Hey Momo" were reliably detected, even in moderately noisy conditions. This was the most stable part of the entire pipeline.

· General speech transcription was not efficient at all. This was the major failure point of the week:

· Whisper small.en frequently mis-transcribed elderly speech, especially with slower articulation, pauses, or lowered volume.

· Short queries were sometimes transcribed as empty strings or hallucinated phrases. 

· The silence detection was too aggressive at 1.2 s — elderly users often pause mid-sentence to gather thoughts, and the system would cut them off and transcribe only a fragment.

· Raising SILENCE_TIMEOUT to 1.8 s helped slightly but made the system feel sluggish and unresponsive.

· Background noise (fan, traffic) still occasionally triggered false starts.

· Net result: the wake word layer was acceptable, but the STT layer was unreliable and became the first major bottleneck.

Deliverables:

· End-to-end voice capture: wake word → transcription (with mixed reliability).

· Tuning log documenting RMS thresholds and filter responses.

Status: 
Wake word detection OK; speech transcription inefficient and unreliable