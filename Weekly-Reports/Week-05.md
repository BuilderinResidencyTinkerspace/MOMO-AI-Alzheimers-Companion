SOFTWARE STACKSETUP & ENVIRONMENT CONFIGURATION

Phase: 
Environment Setup & Toolchain Installation

Objective: 
Establish the software foundation on the Raspberry Pi 5 — Python virtual environment, audio subsystem, and model runtimes.

Activities:

· Created the project directory ~/alzheimer-companion/ and set up a Python virtual environment (.venv).

· Installed and configured Ollama for local LLM inference.

· Pulled and tested Qwen 2.5 1.5B as the base LLM; created a custom model alzheimer-companion:latest using a companion.md system prompt.

· Installed Vosk (vosk-model-small-en-us-0.15, ~40 MB) for wake-word detection and unzipped it into the working directory.

· Installed faster-whisper (small.en) for speech-to-text.

· Installed Piper TTS with the en_US-amy-medium voice model (.onnx + .json).

· Installed PortAudio dependencies: sudo apt install libportaudio2 portaudio11-dev.

· Verified audio routing with amixer -c 2 sset 'Mic' 6 (AGC off, level 6).

· Built a mic diagnostic script (test_mic.py) to confirm capture at 44100 Hz.

Issues encountered:

· Initial LLM inference with Qwen 2.5 1.5B was noticeably slower than expected on the Pi 5 — responses took several seconds even for short prompts, and this was before STT and TTS were in the pipeline.

· Memory pressure was observed when Ollama loaded the model alongside Whisper; the system occasionally thrashed when both were active.

· Piper TTS worked technically, but with no speaker wired, output could only be verified as a WAV file — no real confirmation of audio quality.

Deliverables:

· Working virtual environment with all dependencies.

· test_mic.py confirming live audio capture.

· Custom Ollama model responding to prompts (albeit slowly).

Status: 
Toolchain technically operational, but performance and output-verification gaps already evident.
<img width="720" height="1280" alt="WhatsApp Image 2026-09-18 at 6 24 27 PM" src="https://github.com/user-attachments/assets/eb13d8f2-6c40-42a8-a6d2-d414d564f25c" />
<img width="720" height="1280" alt="WhatsApp Image 2026-09-18 at 6 24 30 PM" src="https://github.com/user-attachments/assets/4942325a-9993-490b-9854-2ec8ba76ed60" />
<img width="720" height="1280" alt="WhatsApp Image 2026-09-18 at 6 24 33 PM" src="https://github.com/user-attachments/assets/f272f7e9-c7d4-4ae2-9430-d2631c5bd0da" />
<img width="900" height="1600" alt="WhatsApp Image 2026-09-18 at 6 24 34 PM" src="https://github.com/user-attachments/assets/12e267c9-8285-4c01-99e8-7ac1596f730f" />
<img width="1280" height="720" alt="WhatsApp Image 2026-09-18 at 6 24 36 PM" src="https://github.com/user-attachments/assets/1437d6d1-ebce-4fbf-9e0b-725f7ccd231d" />




