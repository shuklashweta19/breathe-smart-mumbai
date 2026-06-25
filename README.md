# Breathe Smart Mumbai

**An AI-Powered Air Quality Health Advisory Assistant for Mumbai Commuters**

Submitted as part of the 1M1B AI for Sustainability Virtual Internship, in
collaboration with IBM SkillsBuild & AICTE.

---

## Status

This repository documents a **conceptual prototype**: a complete system
design, prompt engineering, RAG pipeline logic, and a sample multi-turn
conversation demonstrating expected assistant behavior. It is not a
deployed application — there is no live backend or hosted demo at this
stage. Everything here is the design and reasoning layer that a working
build would implement.

---

## Problem

Mumbai's 20+ million residents, including 7.5 million daily local train
commuters, breathe dangerously polluted air, with AQI often exceeding 200
in zones like Chembur, Govandi, Mahul, and Dharavi. SAFAR-India and CPCB
publish AQI numbers but offer no personalized, actionable guidance. A
commuter with asthma traveling through Govandi or Kurla has no way to know
if today is safe to travel, whether to wear a mask, or when to avoid peak
pollution windows.

**Breathe Smart Mumbai** solves this by turning raw AQI data into
personalized, route-specific, conversational health advisories for every
commuter, in their own language.

---

## SDG Alignment

- **Primary:** SDG 3 — Good Health & Well-being (Targets 3.4, 3.8, 3.9, 3.d)
- **Secondary:** SDG 11 — Sustainable Cities and Communities (Targets 11.2, 11.6, 11.b)

---

## How It Works (RAG Architecture)

1. **User query** — commuter describes their route and health profile
   (e.g., "Chembur to BKC, 6 PM, I have mild asthma").
2. **Live data retrieval** — pulls current AQI readings from SAFAR-India /
   CPCB for every zone along the route.
3. **Knowledge base retrieval** — semantic search over WHO air quality
   guidelines, MPCB bulletins, and Mumbai-specific seasonal pollution
   patterns.
4. **Context assembly** — retrieved facts + user health profile are
   merged into a single structured prompt.
5. **Generation (IBM Granite via watsonx.ai)** — produces a personalized,
   conversational advisory: AQI summary → risk assessment →
   Go/Caution/Avoid recommendation → specific precautions.
6. **Delivery** — response is returned with a color-coded AQI badge, in
   the user's preferred language (English / Hindi / Marathi).

See `architecture/system_architecture_diagram.png` for the full data flow.

---

## Why RAG (Not a Standalone LLM or Classifier)

| Approach | Verdict |
|---|---|
| Fine-tuned classification model | Rejected — too rigid for open-ended, conversational health queries |
| Standalone LLM (prompt engineering only) | Rejected as standalone — cannot know today's live AQI, risks hallucinating figures |
| **Retrieval-Augmented Generation (RAG)** | **Selected** — grounds every response in live, verifiable AQI data while still reasoning conversationally about personal health risk |

---

## Technologies Used

- Retrieval-Augmented Generation (RAG)
- IBM Granite Foundation Models
- watsonx.ai (orchestration & governance)
- watsonx.data (vector store for semantic retrieval)
- Prompt engineering (structured system prompts — see `/prompts`)
- Real-time API integration: SAFAR-India, CPCB AQI feeds
- Multilingual conversational AI (English, Hindi, Marathi)

---

## Repository Contents

```
breathe-smart-mumbai/
├── README.md                                   This file
├── docs/
│   └── Breathe_Smart_Mumbai_Project.docx       Full project document (all 10 sections)
├── prompts/
│   ├── system_prompt_health_advisory.txt       Main RAG generation prompt (Granite)
│   └── retrieval_summarization_prompt.txt      AQI zone retrieval/summarization prompt
├── architecture/
│   └── system_architecture_diagram.png         End-to-end system architecture
└── sample-conversation/
    └── sample_chat_transcript.md               3-turn sample advisory conversation
```

---

## Target Users

Local train & BEST bus commuters, asthma/heart condition patients (~1.2M
in Mumbai), elderly citizens (60+), two-wheeler riders, outdoor
construction workers (500,000+), street vendors, and school
children/parents in high-pollution zones (Chembur, Govandi, Kurla,
Mankhurd). Secondary users: MCGM and MPCB officers, NGOs, and
pulmonologists.

---

## Responsible AI

- **Fairness** — equal-quality Marathi/Hindi support, free core access, WhatsApp interface for non-smartphone users.
- **Transparency** — every advisory cites its SAFAR/CPCB source station; stale data (>3 hrs old) is explicitly flagged.
- **Ethics** — never fabricates AQI values; escalates to MCGM helpline (1916) above AQI 300; no medical diagnosis overreach.
- **Privacy** — health profiles stored on-device by default; anonymous session mode; no continuous GPS tracking.

Full details in the project document, Section 8.

---

## Expected Impact

Designed to reach a share of Mumbai's 7.5 million daily rail commuters and
1.2 million asthma patients with free, personalized air quality alerts
that don't currently exist. Aims to increase mask/precaution uptake,
reduce avoidable acute respiratory and cardiac events during severe
pollution spikes, and give outdoor workers realistic harm-reduction
guidance instead of impractical "stay indoors" advice.

---

## License / Attribution

Built as a student project for the 1M1B AI for Sustainability Virtual
Internship (IBM SkillsBuild & AICTE). Air quality data sources (SAFAR-India,
CPCB, MPCB, MCGM) are referenced as public data providers; this repository
does not redistribute their data.
