LLM INTERATION & RESPONSE GENERATION

Phase: 
Intelligence Layer

Objective: 
Connect the transcribed user query to the local Qwen 2.5 1.5B model and generate companion-style responses.

Activities:

· Wired faster-whisper transcription output directly into the Ollama API call for alzheimer-companion:latest.

· Crafted and refined companion.md system prompt to produce:

  · Warm, patient, reassuring tone.

  · Short, simple sentences suitable for memory difficulties.

  · No emoji (compatibility with e-paper).

· Tuned generation parameters (temperature, max tokens) for fast local inference on the Pi 5.

· Handled edge cases: 
empty transcriptions, unclear input, and repeated questions (each turn treated as a fresh conversation at this stage).

· Verified the full chain: 
speech → text → LLM → text response.

Critical findings:

· The 1.5B model was a major limiting factor. Even with a carefully written system prompt, Qwen 2.5 1.5B frequently:

  · Gave generic or repetitive responses that didn't feel personalized.

  · Lost track of context within a single turn when the input was long or garbled.

  · Produced responses that were too long for the e-paper's 5-line pagination, requiring aggressive truncation.

  · Occasionally hallucinated facts or gave advice that was not appropriate for someone with memory difficulties.

· When STT fed garbage or empty text into the LLM, the model would often generate confused or nonsensical replies, compounding the upstream failure.

· End-to-end latency from end-of-speech to first LLM token was often 5–10 seconds, which is too slow for natural conversation, especially for an elderly user who may forget what they asked.

· Running the LLM alongside Whisper and Piper on the Pi 5 caused thermal throttling during extended sessions, further degrading response time.

Deliverables:

· companion_voice.py — integrated mic + wake word + Qwen pipeline.

· Prompt engineering notes for companion.md.

Status: 
LLM responses generating, but quality and latency not acceptable for the intended use case. The 1.5B model size is a fundamental constraint.
<img width="720" height="1280" alt="WhatsApp Image 2026-09-18 at 6 48 52 PM" src="https://github.com/user-attachments/assets/32b20c05-6964-4971-b04d-9b02f76d2397" />
