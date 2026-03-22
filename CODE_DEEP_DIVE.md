# MIMO Guardian: Code Deep Dive & Logic Architecture 🧠

While our Functional MVP is compiled into a single Zero-Dependency Edge Application (`index.html`) for maximum portability and rapid deployment, the internal logic is heavily modularized. 

Below is a technical breakdown of the core engines powering MIMO Guardian, directly referencing the executed logic in our codebase.

---

## 1. The G-Force Fall Detection Engine 📉
Instead of relying on simple, highly inaccurate threshold triggers, we utilize the native `DeviceMotionEvent` API to calculate the absolute G-Force vector. The algorithm relies on a specific physical signature: **Weightlessness (Freefall) followed by High Impact**.

```javascript
// Snippet: Core Physics Calculation inside handleMotion()
const acc = e.accelerationIncludingGravity;
const total = Math.sqrt(acc.x**2 + acc.y**2 + acc.z**2); // Absolute vector
const gForce = (total/9.8).toFixed(1);

// State Machine Logic for False Alarm Filtering
if (state.phase === 'idle' && total < 4.0) { 
    // Stage 1: Freefall detected (Weightlessness)
    state.phase = 'freefall'; 
    state.timestamp = now; 
} else if (state.phase === 'freefall') {
    if (now - state.timestamp > 1000) { 
        // Reset if impact doesn't happen within 1 second
        state.phase = 'idle'; 
    } else if (total > 25.0) {
        // Stage 2: High Impact confirmed immediately after freefall
        state.phase = 'impact';
        core.triggerAlarm("Fall Detected");
        state.phase = 'idle';
    }
}
```
**Enterprise Value:** Executing this State Machine locally on the Edge CPU means ZERO latency, complete privacy, and no cloud computing costs for continuous sensor monitoring.

---

## 2. Continuous Edge Voice Recognition 🎙️
To allow hands-free SOS triggering (Zero-Touch interface), the system uses the `webkitSpeechRecognition` API. It runs continuously in the background, consuming minimal battery, waiting ONLY for specific wake words.

```javascript
// Snippet: Wake-Word Engine Logic
const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
core.recognition = new SpeechRecognition();
core.recognition.continuous = true;  // Keep listening in background
core.recognition.interimResults = true;

core.recognition.onresult = (event) => {
    const transcript = Array.from(event.results).map(r => r[0].transcript).join('').toLowerCase();
    
    // Keyword Matching Trigger
    CONFIG.TEXT[app.lang].triggers.forEach(word => {
        if (transcript.includes(word)) { 
            core.triggerAlarm(word); // Trigger protocol if matched
        }
    });
};
```

---

## 3. Webhook Payload Dispatcher & GPS (Serverless Routing) 📡
We avoid heavy backend databases for the MVP. Instead, we use a Serverless Webhook architecture (Make.com Gateway) to instantly route the SOS payload to the HSE Dashboard, complete with precise and secure GPS coordinates.

```javascript
// Snippet: Payload Dispatcher & GPS Logic
navigator.geolocation.getCurrentPosition((position) => {
    const lat = position.coords.latitude;
    const long = position.coords.longitude;
    const mapLink = `[https://www.google.com/maps?q=$](https://www.google.com/maps?q=$){lat},${long}`; // Secure Maps Routing

    fetch(CONFIG.WEBHOOK_URL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
            alert: reason,
            user: app.user,
            contact: app.emergencyContact, 
            lang: app.lang,
            location: mapLink,
            ai_summary: statusMsg   // Contains patient's vocal response to AI
        })
    }).then(() => {
        ui.toast("✅ Sent!");
        core.provideFirstAid();
    });
});
```
**Security Note:** The payload is assembled entirely on the client side and dispatched directly via a secure POST request, minimizing data interception risks (Privacy by Design).

---

## 4. Edge AI Medic Integration 🤖
Once the emergency is validated, the system triggers the ElevenLabs Conversational AI widget to perform primary medical Triage, speaking directly to the incapacitated user to gather status details before help arrives.

```html
<!-- Snippet: ElevenLabs Widget Injection -->
<elevenlabs-convai agent-id="agent_5601kfrvwrrxfxgafkezx421fe01"></elevenlabs-convai>
```
**Clinical Value:** Reduces the panic of the isolated worker and provides the HSE dashboard with immediate, conversational triage data (e.g., "My leg is broken") rather than just a silent alarm.
```
