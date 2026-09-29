# Threat Analysis of an Agentic Filler Pipeline

A course project applying STRIDE-AI and the OWASP Top 10 for Agentic Applications to a two-tier agentic pipeline for real-time digital-human interaction.

Coursework for a Data-Intensive Architectures course, MSc in Computer Security ("Sicurezza Informatica"), University of Milan, academic year 2025/2026, following a project outline by Prof. Ernesto Damiani ("Threat Analysis of an Agentic Filler Pipeline for Real-Time Digital Human Interaction").

**Scope, stated plainly:** this is a threat-modeling exercise on an existing project design, not new system-building or original attack research. The AIVSS/AARS scores are the author's own analyst estimates — a simplified arithmetic mean, without the agentic multiplicative factor of AIVSS v0.8 — not a formal expert-panel evaluation. They should be read as a relative ranking of threats, not as absolute scores comparable to external AIVSS assessments.

## The system under analysis

A two-tier pipeline designed to close the gap between LLM response latency (2–4 minutes for a high-capacity model) and the ~800 ms threshold at which a human notices an unnatural pause:

```
STT → Fast-Brain → Deep-Brain → TTS → Lip-Sync → WebRTC
```

- **Fast-Brain:** a distilled LLM (~3B parameters) that reacts to the first 10–15 transcribed tokens and emits filler behaviors (gestures, gaze, short verbal fillers) within 0–3 seconds.
- **Deep-Brain:** a full LLM with RAG and persistent session memory, composing the substantive answer in 2–4 minutes.
- **Swap Manager:** hands off between filler and substantive output so the user perceives one continuous interlocutor.

## Method

1. **Asset model (STRIDE-AI):** an asset-centric adaptation of Microsoft's STRIDE for AI/ML systems (Mauri & Damiani, 2022), mapped to CIA³-R properties (Confidentiality, Integrity, Availability, Authenticity, Non-Repudiation, Authorization). Six Data assets (D1–D6) and five Model assets (M1–M5) were identified.
2. **Threat identification:** instead of a per-asset FMEA, threats were identified directly from the **OWASP Top 10 for Agentic Applications 2026** (ASI01–ASI10), all ten of which are covered.
3. **Risk quantification:** OWASP AIVSS v0.8 and its Agentic AI Risk Score (AARS), with the simplification noted above.
4. **Mitigations:** organized into seven security precautions from the course (least agent privilege, human-in-the-loop guardrails, sandboxing, audit trails, supply-chain control, continuous monitoring, periodic AIVSS audits), prioritized on an impact/effort plane.

## Key findings

The highest-priority threats identified:

| Threat | Description | AARS |
|---|---|---|
| ASI01 | Goal hijacking via prompt injection on the first tokens (D2) | 8.5 |
| ASI04 | Supply-chain attack on third-party components | 8.2 |
| ASI06 | Progressive context poisoning via audio deepfakes | 7.9 |
| ASI03 | Privilege abuse via crafted instruction injection (M3) | 7.8 |

Only ASI01 exceeds the paper's own threshold of 8, marking goal hijacking as the most critical threat for this architecture.

On the impact/effort plane (Section 4.8): continuous monitoring, supply-chain control, and periodic AIVSS audits offer the best cost/benefit ratio — mostly process and observability work, no structural pipeline changes, while still covering a broad subset of high-priority threats. Guardrails and human-in-the-loop review remain a necessary strategic investment despite high implementation effort (constrained by the Fast-Brain's 0–3 second latency budget), because they are the only direct defense against ASI01. The analysis also surfaces an indirect effect: guardrails built for filler coherence and prompt-injection blocking also reduce ASI09 (Human-Agent Trust Exploitation) risk, even though that was not their primary design goal.

## Limitations

- AIVSS/AARS scores are analyst estimates, not a formal expert-panel evaluation, and use a simplified scoring formula (see Scope above).
- The residual-risk estimates after mitigation (Section 4.8 / Figure 4.2) are illustrative, not a new formal AIVSS assessment.
- No red-teaming or empirical testing was performed; the paper explicitly recommends a structured red-team exercise against the OWASP Agentic Top 10 test cases as future work.

## Contents

```
.
├── paper_it.pdf   # Full write-up (Italian)
└── README.md
```

## Author

Marco Agnoli, [LinkedIn](https://www.linkedin.com/in/marcoagnoli/)
