# 🧓📱 WhatsApp Voice-Based Cab Booking Assistant

A voice-first WhatsApp assistant that helps elders book cabs easily by speaking naturally and selecting locations

This project was inspired by a real problem: my mother finds modern cab apps confusing, especially typing addresses and choosing locations on maps. This assistant removes those steps by letting users send a voice note on WhatsApp and then confirming the pickup location visually.

# ✨ What this does

🎤 User sends a voice note on WhatsApp (Hindi / Hinglish supported)

🧠 Voice is transcribed and cleaned (filler words removed)

📍 Nearby places are searched using location context

🖼️ The top matches are returned as images (location Images / Static Maps)

✅ User confirms the correct place

🚕 The flow can redirect into a cab-booking app with the selected location

No typing. No app installation. Just WhatsApp.

# 🧩 Why this is interesting (beyond a demo)

This is not just an automation:
- It handles real UX constraints of WhatsApp
- It manages conversation state safely
- It works around platform limitations (message ordering, media delivery)
- It’s designed specifically for elder-friendly interaction

# 🏗️ High-level Architecture

```
WhatsApp (Twilio)
   ↓
Voice Message
   ↓
Speech-to-Text (Deepgram)
   ↓
Text Normalization (Hindi / Hinglish)
   ↓
Google Places Search
   ↓
Image Generation (Places Photos / Static Maps)
   ↓
WhatsApp Image Responses
   ↓
User Confirmation
   ↓
Send deep links for Uber/Ola rides
```
Location Context

If the user has previously completed a booking, the last confirmed pickup latitude/longitude is reused as context for future searches.
This dramatically improves relevance without asking the user for location again.


# 🔑 Key Design Decisions
**1. Voice-first, not chat-first**

Typing is the biggest barrier for elders. The entire flow is optimized for voice input + visual confirmation.

**2. Location as context, not constraint**

Instead of forcing nearest-distance ranking, the last known drop location is used to bias results at the city/area level, improving recognition over strict proximity.

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
- State Storage: n8n Data Tables (session + last drop location)

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
