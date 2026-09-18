TTS, THREADING & INTERRUPTION HANDLING

Phase: 
Audio Output & Concurrency

Objective:
Add Piper TTS, parallelize display and speech, and implement instant cancellation.

Activities:

· Integrated Piper en_US-amy-medium to synthesize responses into /tmp/momo_tts.wav.

· Ensured speech always plays as one continuous utterance, even when the reply spans multiple display pages.

· Implemented parallel daemon threads for paging and TTS inside update_display(), joined at completion.

· Designed and implemented the stop_event (Option B) shared cancellation mechanism:
 
 · Any layer can trigger stop_speaking_and_paging().

 · Instantly cancels both TTS playback and e-paper paging.

· Prepared the one-line switch (PLAY_AUDIO = True + APLAY_COMMAND) to activate speaker output once hardware is wired.

· Wrote the daily run routine and optional run.sh launcher.

· Added --typed mode for mic-free operation.

Critical findings:

· Text-to-speech (Piper) was found to be working well. This was one of the few components that performed reliably:

  · Piper en_US-amy-medium consistently produced clear, natural-sounding WAV output.

  · Synthesis was fast enough to keep up with the pipeline — TTS generation itself was not a bottleneck.

  · The voice was intelligible and appropriately paced, making it suitable for an elderly listener.

  · Even when the LLM produced poor text or the STT fed garbled input, Piper faithfully and clearly read out whatever it was given — the TTS layer itself was never the problem.

  · This confirmed that once a speaker is wired, the audio output side should work as intended, unlike the STT and LLM layers.

· No speaker was wired, so TTS output could only be verified as a WAV file — but the WAV files were consistently good.

· Threading introduced race conditions during testing — occasionally the display would finish paging before TTS started, or vice versa, causing desynchronization.

· The stop_event worked for cancellation, but because the LLM and STT were slow, barge-in felt unresponsive — the user would say "Momo" and wait several seconds before anything actually stopped.

· Piper TTS was reliable, but it was synthesizing low-quality LLM output (Week 8) and mis-transcribed STT input (Week 6), so the audio was often a faithful reading of nonsense.

Deliverables:

· companion_voice.py with full pipeline: wake word → STT → LLM → e-paper + Piper WAV.

· run.sh single-command launcher.

· Documented tuning knobs (SILENCE_TIMEOUT, SILENCE_RMS_THRESHOLD, MAIN_MODEL_SIZE, PAGE_DISPLAY_SECONDS, PLAY_AUDIO, APLAY_COMMAND).

Status: 
Pipeline technically complete. TTS confirmed working well. However, the system is not usable as a real companion due to upstream STT and LLM failures. TTS output unverified through a speaker, but WAV quality is good.