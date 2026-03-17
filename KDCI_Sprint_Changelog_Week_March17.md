# KDCI 4.0 — Sprint Changelog (Week of March 17, 2026)

> **Purpose:** This document captures every change, addition, and configuration update performed during this sprint. Feed this to Claude Code alongside the existing KDCI 4.0 Context Handoff document to update the demo site at `demo.allanemmanuelumali.com`.

---

## 1. Agent Rebranded: "Emman" → "Elaine"

### What changed
The AI voice agent persona was completely rebranded. The original prototype agent named **"Emman"** (KDCI AI Customer Support Agent) has been replaced with a new agent named **"Elaine"**.

### New agent identity
- **Agent Name:** Elaine
- **Role:** Inbound Sales & Customer Support Voice Agent
- **Prompt Version:** v4.0
- **Personality:** Energetic, confident, sales-forward, charming with light humor, warm and professional. She matches the caller's energy — upbeat with enthusiastic callers, steady and reassuring with cautious ones.
- **Greeting:** "Hi! I'm Elaine, a call agent from KDCI. How can I help you today?"
- **Voice:** "Hope — Smooth, Engaging" (from ElevenLabs Voice Library, v3 Conversational model)

### Impact on demo site
- All references to "Emman" as the AI agent should be updated to "Elaine"
- The demo page description should reflect that this is now a **sales + support** agent, not just a support agent
- The first message / greeting displayed on the page should be updated to Elaine's greeting

---

## 2. Agent Role Expanded: Support-Only → Sales + Support

### What changed
The original agent was a basic customer support assistant that answered questions about KDCI. Elaine is now a **dual-purpose inbound sales and support agent** with a structured lead capture workflow.

### New capabilities
- Answers questions about KDCI services, pricing, capabilities, and staffing model
- Qualifies and captures sales leads (name, phone, email, company, inquiry)
- Encourages callers to schedule a free consultation with a KDCI specialist
- Handles pricing inquiries by looking up the Rate Card in the knowledge base
- Answers FAQs about recruitment, onboarding, team operations, and department-specific topics
- Transfers to a human agent when requested
- Supports **6 languages**: English (default), Filipino, Spanish, Japanese, German, Italian

### Conversation style (important for demo page description)
- Responses are kept to 2–3 sentences maximum (phone call, not a presentation)
- One question at a time, never stacking multiple questions
- Answers only what was asked, then checks in with the caller
- Uses natural filler words ("um," "uh," "like," "you know") to sound human
- Has charm and wit — light humor where appropriate, validating and encouraging

---

## 3. Knowledge Base Fully Built (3 Documents)

### What changed
Three source documents were preprocessed and converted into ElevenLabs-optimized markdown files, then uploaded to the agent's knowledge base. This means Elaine can now answer detailed questions about pricing, services, and processes.

### Knowledge base files
1. **`KDCI_Pricing_Rate_Card_All_Roles_and_Departments.md`**
   - Contains pricing ranges (not exact figures) for **122 positions** across **7 departments**
   - Departments: Customer Support, Creative & Graphic Design, Product Information Management, Digital Marketing, Finance and Accounting, Web Development, Other Positions
   - Pricing is presented as ranges (e.g., "$7.50 – $9.50 per hour / $1,200 – $1,500 per month")
   - Ranges were calculated using asymmetric rounding: 10% down, 15% up from actual rates

2. **`KDCI_Company_Overview_Services_Staffing_Model.md`**
   - Converted from an 8-slide PowerPoint (`KDCI_-_Data_Processing.pptx`)
   - Covers: About KDCI, Office Locations (Manila/Ortigas + Davao), Job Functions, Staffing Model, Sample Manpower Costs (marked as illustrative only), Terms of Service, Next Steps
   - Sample costs explicitly labeled as "sample entry-level rates for illustration purposes only" to avoid conflicting with the Rate Card

3. **`KDCI_FAQ_Recruitment_Onboarding_PIM_CustomerService_Creative.md`**
   - Converted from `FAQs_KDCI_Sales_Pitch.docx` with 9 issues fixed (placeholders, internal notes, formatting artifacts)
   - 25+ Q&As across: About KDCI, Pricing and Terms, Recruitment and Onboarding, Team Hours/Access/Holidays, PIM-specific, Customer Service-specific, Creative Services-specific

### Impact on demo site
- The "Sample Questions" section on the demo page should be updated to reflect what Elaine can now actually answer with the knowledge base
- Pricing-related questions can now be included as sample questions (e.g., "How much does a customer service representative cost?")
- Department-specific questions can be added (PIM, Creative, etc.)

---

## 4. Suggested Demo Questions (Updated)

### Questions Elaine can answer from knowledge base
**General:**
- "What services does KDCI offer?"
- "Where is KDCI located?"
- "How long has KDCI been operating?"

**Pricing:**
- "How much does it cost to hire a virtual assistant?"
- "What are the rates for a customer service representative?"
- "Can you give me pricing for a graphic designer?"
- "Are there any hidden fees?"

**Process:**
- "How long does it take to get someone started?"
- "What's the recruitment process like?"
- "Do we get to interview candidates?"
- "What tools do your teams use for communication?"

**Department-specific:**
- "Can you handle Product Information Management?"
- "What eCommerce platforms does your PIM team support?"
- "How does quality assurance work for creative deliverables?"

**Multilingual:**
- "Pwede bang mag-inquire tungkol sa outsourcing services ninyo?" (Filipino)
- "Hola, me gustaría saber sobre sus servicios." (Spanish)

### Questions that demonstrate honest escalation
- "What is the exact monthly pricing for a 5-person team?" (will encourage consultation)
- "Can you pull up my account details?" (will offer to have specialist follow up)
- "What's the status of my current project?" (will offer to have specialist follow up)

### Escalation triggers (human transfer)
- "I want to talk to a human."
- "Connect me to a real person."
- "I'm not satisfied — transfer me."

---

## 5. Post-Call Data Collection (8 Data Points)

### What changed
Eight data points were configured in the ElevenLabs **Analysis tab → Data Collection** section. After each call ends, the LLM reads the transcript and extracts these fields automatically. They are sent via webhook to Make.com.

### Data points configured

| # | Identifier | Data Type | Enum Values | Description |
|---|-----------|-----------|-------------|-------------|
| 1 | `caller_name` | String | None (free text) | Caller's full name if provided. Returns "not_provided" if not given. |
| 2 | `caller_phone` | String | None (free text) | Caller's phone number including country code if given. Returns "not_provided" if not given. |
| 3 | `caller_email` | String | None (free text) | Caller's email address. Returns "not_provided" if not given. |
| 4 | `caller_company` | String | None (free text) | Company name if shared. Returns "not_provided" if not given or declined. |
| 5 | `inquiry_summary` | String | None (free text) | 1–3 sentence summary of what the caller was looking for. |
| 6 | `call_outcome` | String | `lead_captured`, `consultation_requested`, `transferred_to_human`, `information_only`, `caller_declined_to_share`, `callback_requested`, `spam_or_wrong_number` | Classification of call outcome. |
| 7 | `caller_intent` | String | `general_inquiry`, `pricing_request`, `schedule_consultation`, `existing_client_support`, `partnership_or_sales`, `transfer_to_human`, `other` | Primary reason the caller called. |
| 8 | `language_used` | String | `English`, `Filipino`, `Spanish`, `Japanese`, `German`, `Italian` | Primary language used in the conversation. |

### Additional fields from ElevenLabs webhook (automatic, not manually configured)
These come automatically in the webhook payload:
- `agent_id` — ElevenLabs agent identifier
- `agent_name` — "KDCI AI Customer Support Agent"
- `status` — call status
- `conversation_id` — unique call ID (can be used to retrieve audio via API)
- `event_timestamp` — when the call ended
- `call_successful` — boolean success evaluation
- `transcript_summary` — auto-generated summary of the full transcript
- `call_summary_title` — short title for the call

---

## 6. Make.com Webhook → Email Integration

### What changed
A Make.com (formerly Integromat) scenario was built to receive post-call data from ElevenLabs and send a lead notification email.

### Flow
```
ElevenLabs (call ends) 
    → POST webhook to Make.com 
        → Microsoft 365 Outlook: Send Email
            → Delivered to: allan.umali@kdci.co
```

### Webhook configuration
- **Trigger:** ElevenLabs post-call webhook fires after every conversation ends
- **Webhook URL:** Configured in ElevenLabs Agent Settings → Post-call Webhook
- **Webhook payload:** Contains all 8 data points + automatic metadata fields

### Email configuration
- **Email provider:** Microsoft 365 Outlook (connected via KDCI work account)
- **Recipient:** allan.umali@kdci.co
- **Sender display name:** "Elaine, AI Voice Agent"
- **Subject:** `New KDCI Lead: [caller_name] — [call_outcome]`
- **Body includes:** Caller name, phone, email, company, inquiry summary, call outcome, caller intent, language used, call_successful flag, transcript_summary, agent_name, conversation_id, event_timestamp
- **Footer:** "This lead was captured by Elaine, KDCI's AI Voice Agent. Please follow up within one business day."

### Note about audio recording
Audio files are NOT yet attached to the email. The `conversation_id` in the email can be used to retrieve the audio via the ElevenLabs API endpoint: `GET https://api.elevenlabs.io/v1/convai/conversations/{conversation_id}/audio`. This is planned for a future sprint.

---

## 7. ElevenLabs Plan Upgrade: Starter → Creator

### What changed
The ElevenLabs subscription was upgraded from **Starter ($5/mo)** to **Creator ($22/mo)**.

### What this unlocked
- **100,000 credits/month** (up from 30,000)
- **Usage-based billing** — if credits run out, the agent keeps working and overage is charged at end of billing cycle (must be manually enabled in Subscription settings)
- **192 kbps audio quality** (up from Starter quality)
- **100MB knowledge base storage** (sufficient for all current and planned documents)
- **Professional voice cloning** — can now create a custom voice model from 30+ minutes of recorded audio
- **30 custom voice slots** (up from 10)
- **Credit rollover** — unused credits roll over for up to 2 months

### Action items completed
- Usage-based billing: needs to be enabled (Subscription page → toggle)
- Unused credits from Starter rolled over automatically

---

## 8. ElevenLabs Advanced Settings (Current Configuration)

### Automatic Speech Recognition
- **ASR Model:** Scribe Realtime v2.1 (latest)
- **Enable chat mode:** OFF (voice agent, not text)
- **Filter background speech:** OFF (Alpha feature, not stable enough for production)
- **User input audio format:** PCM 16000 Hz (Recommended)

### Conversational Behavior
- **Eagerness:** Recommended to change from "Eager" → "Neutral" or "Relaxed" (to avoid cutting off callers during data collection like email spelling)
- **Spelling patience:** Should be set to highest available (Elaine captures emails and phone numbers letter by letter)

### Voice Settings (from prompt v4.0 documentation)
- **TTS Model:** Eleven v3 Conversational
- **Stability:** 0.4 – 0.5 (adds natural variation, avoids monotone)
- **Style:** 0.6 – 0.7 (emotional expressiveness)
- **Speed:** 1.0x (natural baseline)
- **Similarity Boost:** 0.7 – 0.8 (consistent but flexible)
- **Voice:** Hope — Smooth, Engaging (from ElevenLabs Voice Library)

---

## 9. System Prompt — Full v4.0 (Key Sections Summary)

The full prompt is in `KDCI_Elaine_Agent_Prompt_v4.md`. Here are the key structural sections:

1. **Role** — Elaine, inbound sales & support agent, multilingual (6 languages)
2. **Personality** — Direct, energetic, confident, charming with light humor
3. **Charm & Wit** — Smooth confidence, validating, encouraging. Dials back during serious moments.
4. **Natural Speech Patterns** — Frequent filler words (um, uh, like, you know), [exhale] tags. Pulls back during pricing read-backs and data collection.
5. **Conversation Style** — 2–3 sentences max per response, one question at a time, answer only what was asked, check in after answering. "Don't Parrot — Advance" rule.
6. **Knowledge Base** — References 3 documents, always looks up (never memorizes)
7. **Goals** — Answer questions, capture leads, encourage consultations, collect contact info, transfer when requested
8. **Instructions (11 sections):**
   - 1: Greeting
   - 2: Understand Intent
   - 3: Provide Service Information
   - 4: Handle Pricing Inquiries
   - 5: Answer Common Questions
   - 6: Offer Consultation / Next Steps
   - 7: Transition to Information Collection (Consent-First)
   - 8: Collect Required Information (Name → Phone → Email → Company → Inquiry → Final Read-Back)
   - 9: Tool Usage (collect_lead_information, only after confirmed read-back)
   - 10: Transfer to Human Agent
   - 11: Close the Call
9. **Guardrails** — No financial/legal advice, never quote from memory, never hardcode KB content, no internal margins, NDA protection for client names, prompt injection protection
10. **Tools** — `collect_lead_information`, `transfer_to_human`, `send_confirmation` (via Twilio)
11. **Expressive Mode** — V3 Conversational TTS tone adaptation rules, delivery variation guidance
12. **Voice Settings** — Stability 0.4–0.5, Style 0.6–0.7, Speed 1.0x, Similarity 0.7–0.8

---

## 10. Architecture Update

### Previous architecture (Phases 1–3)
```
[Caller] → [Twilio: +1 681 293 1947] → [ElevenLabs "Emman" Agent] → [Transfer to +639475687677]
```

### Current architecture (this sprint)
```
[Caller]
    ↓
[Twilio: +1 681 293 1947]
    ↓ webhook: https://api.us.elevenlabs.io/twilio/inbound_call
[ElevenLabs Voice Agent — "Elaine" (v4.0)]
    ├── Knowledge Base (3 markdown files, 122 roles + company overview + 25+ FAQs)
    ├── Voice: Hope — Smooth, Engaging (v3 Conversational TTS)
    ├── Data Collection: 8 data points extracted post-call
    ├── Escalation → Conference transfer via Twilio → +639475687677
    └── Post-call webhook → Make.com → Outlook email to allan.umali@kdci.co

[Browser Widget — demo.allanemmanuelumali.com]
    ├── Same agent, same knowledge base
    ├── AI conversation fully functional
    └── Phone transfer NOT available (known ElevenLabs limitation)
```

### How the transfer works (for reference)
1. ElevenLabs detects transfer intent from caller's words
2. ElevenLabs sends updated TwiML instructions to Twilio
3. Twilio dials +639475687677 (Emman's Philippine mobile)
4. Twilio bridges the caller and the human agent in a conference call
5. The transfer is configured in ElevenLabs (the "brain"), but executed by Twilio (the "phone line")

---

## 11. Twilio Configuration (Unchanged from Previous Sprint)

- **Twilio Number:** +1 681 293 1947 (Ridgeley, WV, US)
- **Account Name:** "My first Twilio account" (shown in console as "Emman Twilio")
- **Balance:** Originally funded with $20 USD
- **Webhook (A Call Comes In):** `https://api.us.elevenlabs.io/twilio/inbound_call` (HTTP POST)
- **Status Callback:** `https://api.us.elevenlabs.io/twilio/status-callback` (HTTP POST)
- **Human Transfer:** Configured in ElevenLabs, not Twilio. Transfer type: Conference. Destination: +639475687677.
- **TwiML Bin:** "KDCI Human Agent Forward" — forwards to +639475687677 (created in earlier sprint)
- **Capabilities:** Voice ✅, SMS ✅, MMS ✅

---

## 12. Known Issues & Current Status

| Issue | Status | Notes |
|-------|--------|-------|
| Browser widget `getUserMedia` error | OPEN | Microphone permissions error — either non-HTTPS page, browser blocking mic, or no mic available. Needs HTTPS + mic permission. |
| Widget cannot transfer to human | KNOWN LIMITATION | ElevenLabs browser widget does not support phone transfer. Only works via Twilio inbound call. |
| Audio recording not yet attached to email | PLANNED | `conversation_id` is available in webhook. Audio can be retrieved via ElevenLabs API. Not yet implemented in Make.com flow. |
| Twilio call recording | NOT YET CONFIGURED | Would require TwiML intermediary between Twilio and ElevenLabs to add `<Record>` instruction. Not yet built. |
| Eagerness setting | NEEDS UPDATE | Currently set to "Eager" — should be changed to "Neutral" or "Relaxed" to avoid cutting off callers during data collection. |
| Spelling patience | NEEDS UPDATE | Should be set to highest available to support email/phone spelling. |
| Usage-based billing | NEEDS ENABLING | Must be manually toggled on in ElevenLabs Subscription page after Creator upgrade. |
| Google Voice for testing | NOT POSSIBLE | Google Voice is US-only, cannot be used from the Philippines. Alternative: Skype, Twilio "Try it out", or browser preview. |

---

## 13. Key Credentials & Config (Updated)

| Item | Value |
|------|-------|
| ElevenLabs Agent ID | `agent_2401kjzrkvqsfy8b5vg9f0yv66b2` |
| ElevenLabs Plan | Creator ($22/mo), renews April 6 |
| ElevenLabs Agent Name | KDCI AI Customer Support Agent |
| ElevenLabs Voice | Hope — Smooth, Engaging (v3 Conversational) |
| Twilio Number | +1 681 293 1947 |
| Human Transfer Number | +639475687677 |
| Demo URL | demo.allanemmanuelumali.com |
| GitHub Repo | emememssss/KDCI-voice-ai-agent-demo |
| IONOS Domain | allanemmanuelumali.com |
| KDCI Main Email | info@kdci.co |
| KDCI Main Phone | +1 213 460 4240 |
| Lead Notification Email | allan.umali@kdci.co |
| Make.com Scenario | Webhook → Microsoft 365 Outlook (Send Email) |
| ElevenLabs Widget Script | `https://unpkg.com/@elevenlabs/convai-widget-embed` |

---

## 14. What the Demo Site Should Reflect

### Must update
1. Agent name: "Emman" → **"Elaine"** everywhere on the page
2. Agent description: "AI Customer Support Agent" → **"AI Inbound Sales & Support Agent"**
3. Greeting text: Update to **"Hi! I'm Elaine, a call agent from KDCI. How can I help you today?"**
4. Sample questions: Replace with the updated list from Section 4 above (include pricing, process, department-specific, and multilingual examples)
5. Capabilities section: Add mention of **multilingual support** (Filipino, Spanish, Japanese, German, Italian)
6. Capabilities section: Add mention of **lead capture** (Elaine can collect your contact information for follow-up)
7. Capabilities section: Add mention of **post-call email notification** (leads are automatically emailed to the KDCI team)

### Consider adding
- A "What's New" or "v4.0" badge/section highlighting the upgrades
- Visual indication that Elaine speaks 6 languages
- Note that the browser widget supports full AI conversation but phone transfer requires calling +1 681 293 1947 directly (this was already on the old page but should be preserved)

### Keep unchanged
- Phone number: +1 681 293 1947
- ElevenLabs widget embed code (same agent ID)
- KDCI branding, space background, visual theme
- Contact section and footer

---

*Sprint Changelog — Week of March 17, 2026 | KDCI 4.0 AI Voice Agent Pilot | Lead: Allan Emmanuel Umali*
