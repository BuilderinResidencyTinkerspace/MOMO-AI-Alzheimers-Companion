
HARDWARE IDENTIFICATION AND RESEARCHPhase

: Requirements & Component Selection

Objective:
 
Identify and research all hardware components required to build MOMO — the offline, voice-driven companion robot for an older adult with memory difficulties.

Activities:

· Researched suitable single-board computers for on-device AI inference, selecting the Raspberry Pi 5 for its CPU performance and ability to run quantized LLMs locally.

· Investigated display options and selected the Waveshare 4.3" UART e-paper display for its low power draw, sunlight readability, and simple serial interface (/dev/ttyAMA0, 115200 baud).

· Evaluated microphone options; identified a generic USB microphone (plughw:2,0) as sufficient for far-field wake-word and speech capture.

· Researched speaker options for eventual TTS playback (deferred — TTS initially written to /tmp/momo_tts.wav).

· Confirmed that all components support a fully offline, cloud-free architecture — no data leaves the device.

· Documented power requirements, GPIO/UART pin mapping, and physical assembly constraints.

Concerns raised at this stage:

· The Raspberry Pi 5, while capable, is not a dedicated AI accelerator. Running STT, LLM, and TTS concurrently on CPU is expected to be slow and possibly unreliable, especially under thermal load.

· The 1.5B parameter model size was chosen as a compromise between speed and quality, but it was flagged early that 1.5B may be too small for coherent, contextually appropriate responses to elderly speech.

· No speaker was procured yet, meaning TTS output would not be audible — a significant gap for a voice-driven companion.

Deliverables:

· Bill of Materials (BOM) with part numbers and sourcing links.

· Hardware architecture diagram showing 

Pi 5↔️e-paper ↔️ mic ↔️speakerconnections.

Status:  Components identified and procured, but performance and model-quality risks were already anticipated.

<img width="720" height="1280" alt="WhatsApp Image 2026-09-18 at 5 45 13 PM" src="https://github.com/user-attachments/assets/08bbed80-93d3-45d2-a4dd-d98e3cee60ac" />

<img width="720" height="1280" alt="WhatsApp Image 2026-09-18 at 5 45 13 PM (1)" src="https://github.com/user-attachments/assets/10e73f62-84c3-4e4a-b849-ad2621fad47c" />
