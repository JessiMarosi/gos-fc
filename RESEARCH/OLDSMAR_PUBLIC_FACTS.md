# Oldsmar-Inspired Research  Public Facts (Cited)

## Scope and Safety Notes

- This document records **publicly reported information** about the February 2021 Oldsmar, Florida water treatment facility incident.
- It is **not** an operational analysis, exploit guide, attribution claim, or root-cause determination.
- It intentionally avoids sensitive or actionable technical detail.
- Where public reporting diverges or later reinterpretation exists, those disputes are explicitly labeled.

This file exists solely to define **what is safe to fictionalize from** for GOS-FC validation scenarios.

---

## Source Index (Public, Cited)

Primary advisory:
- CISA Cybersecurity Advisory AA21-042A (dated Feb 12, 2021):  
  https://www.cisa.gov/news-events/cybersecurity-advisories/aa21-042a

Related alert posting:
- CISA Alert (dated Feb 11, 2021):  
  https://www.cisa.gov/news-events/alerts/2021/02/11/compromise-us-water-treatment-facility

Public reporting:
- WIRED (Feb 8, 2021):  
  https://www.wired.com/story/oldsmar-florida-water-utility-hack/
- Chemical & Engineering News / ACS (Feb 12, 2021):  
  https://cen.acs.org/environment/water/water-treatment-plant-hack-affected/99/web/2021/02
- Associated Press (Feb 8, 2021):  
  https://apnews.com/article/hacker-tried-poison-water-florida-ab175add0454bcb914c0eb3fb9588466

Later reinterpretation / dispute:
- CyberScoop (Apr 10, 2023):  
  https://cyberscoop.com/water-oldsmar-incident-cyberattack/
- FOX 13 Tampa Bay (Apr 11, 2023):  
  https://www.fox13news.com/news/hack-of-oldsmar-water-plant-reported-two-years-ago-could-have-been-employee-error

Government / oversight context:
- Senator Mark Warner press release (Feb 17, 2021):  
  https://www.warner.senate.gov/public/index.cfm/2021/2/warner-requests-answers-following-concerning-cyber-breach-on-florida-water-plant
- EPA Office of Inspector General memo (Jul 23, 2021):  
  https://www.epa.gov/system/files/documents/2021-07/_epaoig_notificationmemo_7-23-21_cybersecurity_0.pdf
- Idaho National Laboratory / CyOTE case study  
  (Document dated Dec 31, 2022; posted later to CyOTE site):  
  https://cyote.inl.gov/content/uploads/24/2025/12/CyOTE-Case-Study_Oldsmar.pdf

---

## Verified Public Facts (High Confidence)

### F1  Unauthorized remote interaction was publicly reported
Public officials and multiple contemporaneous reports state that an unauthorized remote interaction affected a control interface at a municipal water treatment facility in Oldsmar, Florida.  
Sources: CISA AA21-042A; WIRED; AP.

---

### F2  A chemical treatment parameter involving sodium hydroxide was reported as changed, then reverted
Multiple public sources report that a sodium hydroxide (lye) level or setpoint was altered from a normal value (commonly reported as ~100 ppm) to a much higher value (commonly reported as ~11,100 ppm), and then corrected.  
Sources: CISA AA21-042A; WIRED; C&EN; AP.

---

### F3  Human observation and intervention was reported as the immediate mitigating factor
Public reporting describes an operator noticing unexpected cursor or control behavior and reversing the chemical parameter change.  
Sources: WIRED; AP.

---

### F4  The incident prompted federal and sector-wide cybersecurity concern
The Oldsmar incident was cited in government communications as a motivating example for increased scrutiny of cybersecurity practices in community water systems.  
Sources: EPA OIG memo; Warner press release (characterization, not technical verification).

---

### F5  The incident narrative later became disputed
Later reporting raised the possibility that the initial characterization as a hack may have been incomplete or incorrect, and that at least part of the event could have resulted from human error rather than malicious remote compromise.  
Sources: CyberScoop; FOX 13 Tampa Bay.

---

## Structured Timeline (Public Reporting)

**Feb 5, 2021 (reported):**
- Unexpected remote interaction observed.
- Sodium hydroxide parameter reportedly altered and reverted.
Sources: CISA AA21-042A; WIRED; AP.

**Feb 812, 2021:**
- Widespread media coverage.
- Federal awareness and response discussions begin.
Sources: WIRED; AP; C&EN.

**Feb 12, 2021:**
- CISA publishes Advisory AA21-042A.
Source: CISA.

**Jul 23, 2021:**
- EPA OIG memo references Oldsmar as a motivating incident for cybersecurity oversight.
Source: EPA OIG.

**Dec 31, 2022 (document date; posted later):**
- INL CyOTE case study documents the incident at a high level.
Source: INL CyOTE.

**Apr 2023:**
- Reporting highlights uncertainty and competing explanations.
Sources: CyberScoop; FOX 13.

---

## Disputed or Unresolved Items (Explicitly Not Facts)

### D1  Exact technical access path and root cause
Public sources do not conclusively establish how the remote interaction occurred or whether it was malicious or accidental.  
Sources: CISA; CyberScoop; FOX 13.

### D2  Treatment-stage impact and downstream exposure
Without details about treatment stage, flow rate, and dosing context, precise impact cannot be computed from public data.  
Source: C&EN.

### D3  Attribution (who, why, from where)
No public source establishes attribution. Attribution is out of scope for GOS-FC regardless.  
Source: WIRED.

---

## Safe Fictionalization Anchors

The following elements may safely inspire fictional scenarios:

- A municipal water treatment context
- Remote interaction observed under ambiguous circumstances
- A treatment parameter changed and reverted
- Human detection preventing escalation
- Conflicting post-incident narratives
- Governance ambiguity under public scrutiny

---

## Do-Not-Assert List

The following must NOT be asserted as factual in scenarios:

- Specific vendor products or configurations
- Exact network architecture
- Named individuals or implied culpability
- Definitive claims of hacking vs. error
- Step-by-step access or exploit details

---

## Companion Research File

Open questions and gaps belong in:
- RESEARCH/OLDSMAR_OPEN_QUESTIONS.md

This file intentionally records only **what is publicly asserted**, not what is inferred.

