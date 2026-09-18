DISPLAY & PAGINATION SYSTEM

Phase: 
Visual Output Layer

Objective: 
Build the e-paper display driver with text pagination and a friendly visual indicator.

Activities:

· Implemented UART communication with the Waveshare 4.3" e-paper at 115200 baud.

· Designed the smiley face as a circle + two filled dots + arc-approximated smile (ASCII-safe, no emoji).

· Implemented text layout:

  · Wrap at 17 characters per line.

  · Max 5 lines per page.

· Built pagination logic: long replies cycle through pages at PAGE_DISPLAY_SECONDS = 5.0 each.

· Stripped emoji from Qwen output (emoji break e-paper ASCII encoding).

· Ran paging and TTS in parallel daemon threads, joined at the end of update_display().

Issues encountered:

· The 17-character line limit is extremely restrictive. Many LLM responses had to be split awkwardly, and some words were broken mid-word, making reading harder for someone with memory difficulties.

· Pagination at 5 seconds per page was found to be too fast for slower reading, but increasing it made the system feel unresponsive and out of sync with TTS.

· Because the LLM was slow (Week 5) and STT unreliable (Week 6), the display layer was often waiting on bad input — so the quality of displayed text was frequently poor, not because of the display code but because of upstream failures.

· No speaker meant the display was the only output channel, putting more pressure on it to be legible — and it wasn't always.

Deliverables:

· companion_display.py — typed-only display version (fallback).

· Working e-paper rendering with pagination and smiley.

Status: 
Display layer functional, but constrained by upstream input quality and slow LLM output.