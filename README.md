# SEBB (Smart Electronic Braille Book) - System Architecture & Firmware Blueprint

**Project Status:** Conceptual Phase (TRL 2) - Hardware Assembly Pending
**Hackathon:** AI Hackathon for People with Disabilities (KSCDR 2026)
**Team:** SEBB

## Overview
SEBB is an affordable, multi-line tactile e-reader equipped with an integrated context-aware AI voice assistant. This repository outlines the software architecture, hardware manifest, and data flow required to transition SEBB from a conceptual design to a functional prototype.

## Hardware Manifest (BOM Draft)
To achieve a low-cost, zero-static power device, the following core components are specified for the prototype build:
1.  **Core Processing:** Raspberry Pi Zero 2 W (Acts as the main hub managing both tactile and AI paths).
2.  **Actuation:** 3D-Printed Electromagnetic Actuators (Custom cam-latching mechanism for zero static power).
3.  **Local Storage:** MicroSD Card (Stores offline digital eBook files and SQLite database).
4.  **Audio Input/Output:** Built-in microphone and integrated small-form-factor speakers.
5.  **User Interface:** Multi-line Braille display interface + Dedicated physical AI Assistant button.

## Software Architecture & Data Flow

The system operates on two parallel paths, managed by a custom Python Object-Oriented Programming (OOP) firmware on the Raspberry Pi:

### 1. The Tactile Path (Offline Reading)
*   **Input:** Offline digital eBook files are read from local storage.
*   **Processing:** The Python firmware reads the text and translates it into Braille signals.
*   **Output:** The Raspberry Pi triggers the electromagnetic actuators, raising the Braille pins on the multi-line display.
*   **Metadata:** An SQLite database is used strictly for metadata (e.g., current file path, active page number, bookmarking) to ensure fast load times and efficient memory usage.

### 2. The AI / Voice Path (Context-Aware Assistance)
*   **Input:** When the user hits a complex word, they press the physical AI button. The microphone captures their spoken question.
*   **Contextual Binding:** The Python firmware extracts the *exact text context* (the paragraph currently displayed on the Braille cells) from the system's memory.
*   **Processing (Cloud):** The Raspberry Pi bundles the voice query and the text context, sending it via Wi-Fi to the **Gemini 1.5 Flash API**.
*   **Output:** Gemini processes the query against the context and returns a highly accurate, conversational Arabic explanation, which is played through the integrated speakers.

## Why this Architecture?
*   **Performance:** Offloading heavy AI processing to the Gemini API allows us to use a low-cost, low-power Raspberry Pi Zero 2 W locally.
*   **Cost-Efficiency:** By relying on 3D-printed latching actuators instead of expensive piezoelectric cells, we drastically reduce the cost per cell.
*   **Autonomy:** The combination of multi-line continuous reading and instant, context-aware AI explanations provides visually impaired students with true 100% independent learning.

---
*Note: This repository currently serves as the technical blueprint and architecture documentation for the SEBB hackathon pitch. Firmware code (Python) and mechanical CAD files will be committed during the active prototyping phase.*
