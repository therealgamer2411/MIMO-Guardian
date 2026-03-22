# MIMO Guardian 🛡️ (Functional MVP)
> **The First Edge-AI Emergency Operating System for HSE & Remote Workers**

[![Status](https://img.shields.io/badge/Status-Live_Functional_MVP-brightgreen.svg)]()
[![Architecture](https://img.shields.io/badge/Architecture-Serverless_Edge_App-blue.svg)]()
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)]()

## 📖 1. Executive Overview
MIMO Guardian is an Edge-Computing SaaS platform designed to protect isolated workers in extreme environments (e.g., Mining and Agriculture). This repository contains our **Functional MVP**. 

Unlike standard UI prototypes, this MVP is a **Serverless Edge Application**. It processes hardware sensor data (G-Force, Voice) entirely locally on the client's device, connecting to Cloud APIs strictly for emergency routing and AI Medic intervention to ensure zero latency and maximum privacy.

## 🚀 2. Live Interactive Demo
You can test the fully functional system directly via your mobile browser:
**👉 [ضع رابط GitHub Pages الخاص بك هنا] 👈**

## ⚙️ 3. How to Test the MVP (Important Instructions for Judges)
To experience the true capability of the MIMO Guardian engine, please follow these steps:
1. **Open the Live Link** on a modern smartphone (Android/iOS).
2. **Grant Permissions:** The system requires **Microphone** (for the AI Medic & Wake Words) and **Location** (for the SOS GPS ping) permissions to function.
3. **Login:** Enter a test name and emergency contact.
4. **Activate Guard:** Click the main activation button to arm the sensors.
5. **Simulate an Emergency:**
   * *Voice Trigger:* Say keywords like "Help", "Doctor", or "Emergency" (Supports Arabic and English).
   * *Fall Trigger:* Click the "Test Fall" simulation chip, or simulate a physical drop to trigger the local G-Force algorithm.
6. **AI Interaction:** The system will initiate a 10-second countdown and ask for your status. You can reply naturally to the AI Medic.
7. **Webhook Routing:** Once the countdown ends, a real-time payload containing your GPS coordinates, name, and transcribed response is dispatched via our secure Webhook gateway.

## 🧠 4. Technical Architecture (Under the Hood)
This single-page application is powered by a robust integration of native web APIs and external webhooks:
*   **Sensor Layer:** `DeviceMotionEvent` for multi-stage G-Force calculation (Freefall -> Impact -> Silence).
*   **Voice Engine:** `webkitSpeechRecognition` for continuous offline keyword listening without battery drain.
*   **Routing Gateway:** Make.com Webhook integration for dispatching the encrypted emergency payload.
*   **AI Medic:** Direct integration with `ElevenLabs Convai` widget (Agent ID: 5601kfrvwrrxfxgafkezx421fe01).

## 👨‍💻 5. The Founding Team
*   **Eng. Mohamed Abdallah** - CEO / CTO & AI Architect
*   **Ahmed Abdelnasser** - QA & Field Testing Lead
*   **Hassan Tarek** - Market Research & Business Dev.
