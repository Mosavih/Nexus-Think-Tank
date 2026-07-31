# Nexus-Think-Tank
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
SECTION 1: VISION AND INFORMATION FLOW
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
DESIGN REQUIREMENTS

| ID  | Requirement                                             | Why it matters                                |
| --- | ------------------------------------------------------- | --------------------------------------------- |
| R1  | Continuously collect information from diverse sources   | Avoid manual monitoring                       |
| R2  | Preserve raw evidence permanently                       | Ensure traceability                           |
| R3  | Convert unstructured information into structured events | Enable reasoning                              |
| R4  | Build cumulative organizational knowledge               | Prevent "starting from zero" every day        |
| R5  | Support specialized analytical perspectives             | Different domains require different expertise |
| R6  | Separate evidence, claims, and knowledge                | Avoid confusing facts with interpretations    |
| R7  | Evaluate forecasts against reality                      | Enable learning                               |
| R8  | Keep humans responsible for publication                 | Maintain accountability                       |
| R9  | Make every module replaceable                           | Technology changes rapidly                    |
| R10 | Minimize daily human effort                             | Respect your 30–60 minute time budget         |

#####

INFORMATION FLOW:
Reality

↓

Evidence

↓

Structured Events

↓

Knowledge Base

↓

Specialized Agents

↓

Competing Claims

↓

Human Review

↓

Published Analysis

↓

Reality unfolds

↓

Evaluation

↓

Knowledge Base updated

######

EVENTS should contain:

ID

Date

Summary

Evidence

Entities

Location

Importance

Confidence

#####

Every published analysis should be stored with [analysis record] fields such as:

Question addressed (e.g., "Will energy shortages worsen this summer?")
Evidence used (links to the underlying structured events)
Key claims
Forecasts, each with explicit probabilities or confidence levels
Underlying assumptions
Publication date
Evaluation status (pending, partially confirmed, contradicted, confirmed)
Post-hoc assessment explaining why the prediction succeeded or failed

#####

CORE OBJECTS

Source

↓

Evidence

↓

Event

↓

Entity

↓

Analysis Record

↓

Forecast

↓

Knowledge

#####

EACH OBJECT HAS A LIFECYCLE

Evidence

Created by:
Collector

Modified by:
Nobody

Deleted?
Never

-------------------

Analysis

Created by:
Analyst Agent

Modified by:
Human

Archived?
Yes

-------------------

Forecast

Created by:
Forecast Agent

Updated?
Never

Evaluated?
After outcome
