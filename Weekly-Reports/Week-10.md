INTEGRATION TESTING, TUNING & DOCUMENTATION

Phase: 

System Validation & Handover

Objective:
 
Validate the complete system under real-world conditions, tune thresholds, document pitfalls, and prepare for speaker deployment.

Activities:

· End-to-end testing with the target user profile (elderly speech, variable volume, pauses mid-sentence).

· Tuned SILENCE_TIMEOUT (1.2 → 1.8 s) to avoid cutting off slow speech.

· Adjusted SILENCE_RMS_THRESHOLD (300 → 400) to reject ambient noise; verified quiet speech still triggers.

· Confirmed mic level reset behaviour after reboot (resets to 16/max → clipping at 32759 → breaks wake detection). Documented the mandatory amixer -c 2 sset 'Mic' 6 on every launch.

· Verified stop_event cancellation latency from wake word mid-reply.

· Documented recurring pitfalls:

  1. Mic level resets to 16 after reboots.

  2. Heredocs + input() don't mix — use separate scripts.

  3. sounddevice requires PortAudio and rejects plughw:2,0 strings (use indices or name substrings).

  4. Vosk needs its model folder unzipped in the CWD.

· Finalized the project summary document covering hardware, software stack, decisions, files, run routine, and tuning knobs.

· Ranked optional future upgrades:

  1. Add speaker (biggest user-visible improvement).

  2. Swap LLM to gemma2:2b (warmer tone, ~2× slower).

  3. medium.en Whisper (better, ~5–8 s/utterance).

  4. Barge-in via wake word mid-reply.

  5. Conversation history.

  6. openWakeWord custom "Momo" model.

  7. Physical button / reminder scheduler.

· Identified what not to chase: bigger Vosk models, non-Piper TTS, LLMs above 3B.

Critical findings from integration testing:

· The system did not perform as intended. While each individual layer worked in isolation, the integrated system was too slow, too inaccurate, and too unreliable for a real elderly user.

· Wake word detection remained the strongest component — this was the one part that consistently worked.

· Text-to-speech (Piper) was confirmed to be working well — clear, fast, natural-sounding WAV output. This was the second reliable component after wake word detection. The problem was never the TTS layer itself, but the poor-quality text it was asked to read.

· Speech transcription was the weakest link. Elderly speech patterns (slow, soft, pause-heavy) were frequently mis-transcribed, leading to cascading failures downstream.

· The 1.5B model was insufficient. It could not reliably generate coherent, contextually appropriate, personalized responses — even with a well-crafted system prompt. The combination of a small model, slow CPU inference, and noisy STT input meant that the majority of interactions produced unsatisfactory results.

· No speaker meant the primary output modality was untested. For a voice-driven companion, this is a critical gap — the project cannot be considered complete without it. However, since Piper TTS was confirmed working at the WAV level, wiring a speaker is a two-line change that should immediately activate good-quality audio output.

· Thermal throttling during extended sessions was a real concern; the Pi 5 was not comfortably handling the concurrent workload.

· The overall conclusion: MOMO is a functional prototype, not a deployable product. Two layers work well (wake word, TTS); two layers are problematic (STT, LLM). Significant work remains on STT accuracy and LLM quality before it can be considered usable by the target user.

Deliverables:

· Fully tested companion_voice.py (with known limitations).

· Project summary and tuning documentation.

· Deployment checklist for speaker activation.

Status: 
Working end to end in a technical sense (wake word → transcription → Qwen → e-paper → Piper WAV), but not performing as intended. Wake word detection is OK; TTS is confirmed working well; speech transcription is inefficient; the 1.5B model limits response quality and system responsiveness. Speaker hardware still pending, but TTS WAV quality is already verified.


Summary Table

Week Phase Key Outcome Status

4 Hardware Identification & Research BOM + architecture diagram 

⚠️ Risks flagged

5 Software Stack Setup Offline toolchain operational 

⚠️ Slow LLM, memory pressure

6 Voice Input Pipeline Wake word OK; STT unreliable 

⚠️ STT is major bottleneck

7 Display & Pagination E-paper rendering + smiley 

⚠️ Constrained by upstream

8 LLM Integration Local Qwen responses

❌ Poor quality, high latency

9 TTS, Threading, Interruption Full pipeline + stop_event 

✅ TTS working well

10 Integration Testing & Docs Validated system + handover 

❌ Not usable as intended


Overall Assessment

What worked well:

· ✅ Wake word detection (Vosk with grammar restriction) — reliable, even in moderate noise.

· ✅ Text-to-speech (Piper en_US-amy-medium) — confirmed working well: clear, fast, natural-sounding WAV output. The TTS layer was never the bottleneck.

· E-paper display rendering and pagination — technically functional.
· The stop_event cancellation architecture — worked, though felt slow.

What did not work as intended:

· ❌ Speech-to-text for elderly speech — inefficient, inaccurate, and the primary failure point.

· ❌ The 1.5B LLM — too small to generate coherent, contextually appropriate responses; too slow on the Pi 5 for natural conversation.

· ❌ Integrated system performance — thermal throttling, memory pressure, and 5–10 s end-to-end latency.

· ❌ No speaker — the primary output modality was never tested in real use, though TTS WAV quality was verified.

Honest conclusion: MOMO is a working prototype with significant limitations. Two of the four major layers (wake word detection and TTS) work well. 
The other two (STT and LLM) do not perform adequately for the intended use case. The project demonstrates the feasibility of a fully offline, voice-driven companion on a Raspberry Pi 5, but it is not yet ready for deployment to an elderly user with memory difficulties. The next phase must focus on improving STT accuracy, upgrading to a larger LLM (or accepting a hybrid approach), and completing speaker integration — before any real-world testing can begin.

Next milestone (Week 11+): Wire the speaker, flip PLAY_AUDIO = True, and run a realistic evaluation with the target user — with honest metrics on transcription accuracy, response latency, and response quality. Since TTS is already confirmed working, this step should immediately produce audible output — but the quality of that output will depend entirely on fixing the STT and LLM layers. Only then can the project claim to be a usable companion.