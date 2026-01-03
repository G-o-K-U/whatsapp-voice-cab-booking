# 🧓📱 WhatsApp Voice-Based Cab Booking Assistant

A voice-first WhatsApp assistant that helps elders book cabs easily by speaking naturally and selecting locations

This project was inspired by a real problem: my mother finds modern cab apps confusing, especially typing addresses and choosing locations on maps. This assistant removes those steps by letting users send a voice note on WhatsApp and then confirming the  location visually.

# ✨ What this does

🎤 User sends a voice note mentioning drop location on WhatsApp (Hindi / Hinglish / English supported)

🧠 Voice is transcribed and cleaned (filler words removed)

📍 Nearby places are searched using the user's location context

🖼️ The top matches are returned as images (Location Images / Static Maps)

✅ User confirms the correct drop location

🚕 The flow can redirect into a cab-booking app with the selected location

No typing. No app installation. Just WhatsApp.

<img width="250" height="1000" alt="voice flow1" src="https://github.com/user-attachments/assets/64e45fb3-a911-4676-84d5-f90f9186f8b9" />   <img width="250" height="1000" alt="voice flow2" src="https://github.com/user-attachments/assets/0de2a27e-7d7a-4b2a-9e5e-27530c6f0c08" />   <img width="250" height="1000" alt="cab booking page" src="https://github.com/user-attachments/assets/2521a97e-935b-46af-a354-e0697da7139a" />


# 🏗️ High-level Architecture

```
WhatsApp (Twilio API)
   ↓
Voice Message
   ↓
Speech-to-Text (Deepgram API)
   ↓
Text Normalization (Hindi / Hinglish)
   ↓
Google Places Search API
   ↓
Image Retrieval (Places Photos / Static Maps)
   ↓
WhatsApp Image Responses
   ↓
User Confirmation
   ↓
Send deep links for Uber/Ola rides
```
Location Context

The last confirmed  latitude/longitude, if available, is reused as context for future searches.
This dramatically improves relevance without asking the user for their location.


# 🔑 Key Design Decisions
**1. Voice-first, not chat-first**

Typing is the biggest barrier for elders. The entire flow is optimized for voice input + visual confirmation.

**2. Location as context, not constraint**

Instead of forcing nearest-distance ranking, the last known  location is used to bias results at the city/area level, improving recognition over strict proximity.

**3. Explicit state handling**

Each voice request creates a fresh session.
If a user sends a new voice note without replying to the previous options, old data is safely overwritten to avoid accidental bookings.

**4. Visual confirmation over text precision**

Rather than asking users to confirm text addresses, the system shows images and maps, which are much easier to recognize.


# 🛠️ Tech Stack

- WhatsApp: Twilio WhatsApp API
- Workflow Orchestration: n8n (used for rapid prototyping)
- Speech-to-Text: Deepgram (Hindi + Hinglish)
- Places & Maps: Google Places API, Places Photos, Static Maps
- State Storage: n8n Data Tables (session + last  location)

> Note: n8n was intentionally used to prototype quickly. The system is designed to be easily portable to a lightweight serverless backend (e.g., Cloudflare Workers).

# 🧪 Edge Cases Handled

- Users sending multiple voice notes without replying
- Out-of-order WhatsApp media delivery
- Missing place photos (fallback to Static Maps)

# 📂 Repository Contents

```
.
├── workflow.json        # Exported n8n workflow
├── README.md            # Project overview (this file)
└── assets/              # (Optional) screenshots / diagrams
```

The workflow.json can be imported directly into n8n for exploration.

# 🚀 Future Improvements
- Replace n8n with a serverless backend for lower cost
- Add optional “home” and “frequent places” shortcuts
- Auto-select when only one strong match exists

# 💡 Why I built this

This project was built for a real family use case, not as a tutorial or demo.
It reflects how I approach problems at the intersection of UX, data, and engineering — by focusing on constraints, failure modes, and the end user’s comfort.

# 📬 Feedback

Happy to hear feedback, suggestions, or ideas to extend this further.
