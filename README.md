# Nexus-Think-Tank
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
#  SECTION 1: VISION AND INFORMATION FLOW
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

This project aims to develop a semi-automated self-improving system that provides financial, political, military, governance, and technology analyses and forecasts (posts). Relevant data will be extracted and saved in the database in a structured format. Specialized agents will subsequently use the data to provide posts. Each post will be saved in the database with a dedicated identifier. New evidence (data) will validate the previous posts, and their status will be updated, turning them into knowledge. The knowledge will be used to improve the performance of various agents.

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

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
SECTION 2: DESIGN REQUIREMENTS
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

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

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
#  SECTION 3: INFORMATION MODEL
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

##  3.1 CORE OBJECTS

###  3.1.1 Source

A Source describes where information originates, not what happened.
Examples: Reuters, ISNA, Central Bank of Iran, Parliament website, Telegram channel, X account, Government report, Academic paper
The Source says nothing about the event itself.

Responsibilities

A Source should answer:

Where did this information come from?
How trustworthy is it?
What type of source is it?
How frequently should it be checked?

**Attributes**

- Source

- ID

- Name

- Type(news agency, government, social media, database, research)

- Language

- Country

- URL

- Reliability score

- Political affiliation (optional)

- Collection method (RSS, API, scraping)

- Update frequency

- Status (active/inactive)

###  3.1.2 Evidence

Evidence represents a single observed piece of information exactly as obtained from a source.

Example

Reuters reports:

Iran announced the commissioning of two new gas processing facilities.

That article becomes one or more Evidence objects.

Responsibilities

Evidence should answer:

What exactly was observed?
Who reported it?
When?
How confident are we that the source reported this?

**Attributes**

- Evidence

- ID

- Source

- Collection time

- Publication time

- Original title

- Original text

- Original URL

- Language

- Media type

- Confidence

- Hash

- Processing status

###  3.1.3 Event

Multiple Evidence objects can describe one Event.

Purpose

Represent one occurrence in reality.

Example

Parliament approved
the 2027 budget.

That is one Event.

Ten newspapers may describe it differently.

Still one Event.

Responsibilities

An Event should answer

What happened?
Where?
When?
Who was involved?

**Attributes**

- Event

- ID

- Title

- Summary

- Start time

- End time

- Location

- Importance

- Confidence

- Evidence IDs

- Entity IDs

- Category

- Status

###  3.1.4 Entity

Entities are stable things.

Examples

People

Organizations

Companies

**Attributes**

- Entity

- ID

- Name

- Type

- Aliases

- Description

- Relationships

- Importance

- Confidence

###  3.1.5 Analysis Record

An Analysis Record should never overwrite reality.

Instead it represents

"At this moment,
given this evidence,
this analyst believes..."

**Attributes**

- Analysis Record

- ID

- Question

- Author (agent or human)

- Date

- Evidence used

- Assumptions

- Reasoning chain

- Claims

- Alternative hypotheses

- Confidence

- Forecast IDs

###  3.1.6 Forecast

A Forecast should answer

What?

Probability?

Time horizon?

Conditions?

Confidence?

Evaluation status?

Example

Probability

70%

Prediction

Fuel prices increase

Deadline

3 months

###  3.1.7 Knowledge

Knowledge should instead represent validated relationships about how the system believes the world works.

For example:
Economic sanctions

↓

usually reduce

↓

construction imports

Or

Currency depreciation

↓

often precedes

↓

construction inflation

Knowledge is not

"Sanctions happened."

Knowledge is

"Historically, sanctions tend to produce X under conditions Y."

**Attributes**

- Knowledge

- ID

- Statement

- Supporting analyses

- Supporting forecasts

- Supporting events

- Supporting evidence

- Confidence

- Last updated

- Contradicting evidence

- Status


##  3.2 OBJECT LIFECYCLE

```mermaid
flowchart TD
    A[New article appears]
    B{Source already known?}
    C[Create Source]
    D[Evidence created]
    E{Duplicate?}
    F[Discard duplicate]
    G{Existing Event?}
    H[Update Event]
    I[Create Event]
    J[Link Entities]
    K[Knowledge Base Updated]
    L{Need Analysis?}
    M[Analysis Generated]
    N[Human Review]
    O[Publication]
    P[Forecast Stored]
    Q[Reality Unfolds]
    R[Evaluation]
    S[Knowledge Updated]

    A --> B
    B -- No --> C
    B -- Yes --> D
    C --> D
    D --> E
    E -- Yes --> F
    E -- No --> G
    G -- Yes --> H
    G -- No --> I
    H --> J
    I --> J
    J --> K
    K --> L
    L -- Yes --> M
    L -- No --> Q
    M --> N
    N --> O
    O --> P
    P --> Q
    Q --> R
    R --> S
```

##  3.3 Decision Gates

###  Gate 1 — Collection Gate

Question:
Should this information even enter our system?

Inputs:
- Source
- Retrieved content

Possible outputs:
- Accept
- Reject
- Archive

Decision criteria:
- Is the source trusted?
- Is the content accessible?
- Is it within our scope?
- Is it spam?

###  Gate 2 — Evidence Gate

Question:
Is this evidence sufficiently reliable?

Inputs:
- Raw article
- Source metadata

Outputs:
- Evidence object
- Rejected evidence

Criteria:
- Successful parsing?
- Complete?
- Confidence above threshold?

###  Gate 3 — Event Gate

Question:
Does this evidence describe a new event?

Possible outputs:
- New Event
- Existing Event
- Multiple Events

This is essentially an entity resolution problem.

###  Gate 4 — Priority Gate

Question:
Which events deserve attention?
- Pipeline collects
- 350 news articles
- 95 events

Analysts cannot read 95 events every day.

The system needs to rank them.

Possible criteria:
- Novelty
- Importance
- Confidence
- Potential downstream impact
- Number of affected domains
- Financial significance

Output:
Critical

↓

High

↓

Medium

↓

Low

###  Gate 5 — Routing Gate

Question:
Which specialists should receive this event?
- Economic Agent
- Political Agent
- Military Agent
- Technology Agent
- Governance Agent

Not every event goes to every analyst.

###  Gate 6 — Claim Gate

Question:
Should this analysis become an official claim?

Because not every LLM output deserves to become part of the institutional memory.

This gate could include:
- Critic Agent
- Cross-agent agreement
- Confidence estimation

###  Gate 7 — Publication Gate

Question

Where should this go?

Possibilities:
- Internal knowledge only
- Telegram
- Weekly report
- Monthly report
- Think tank archive

###  Gate 8 — Learning Gate

This is the gate that excites me most.

Question,

Reality has unfolded.

Now what?

Possible outcomes:
- Forecast confirmed
- Forecast partially confirmed
- Forecast contradicted

Then

Update:
- Knowledge
- Forecast calibration
- Agent reliability
- Historical patterns

This is what makes the system adaptive.

##  3.4 Confidence Framework

| Object    | Confidence means                                      |
| --------- | ----------------------------------------------------- |
| Source    | Historical trustworthiness                            |
| Evidence  | Confidence that the extraction is correct             |
| Event     | Confidence that this event representation is accurate |
| Analysis  | Confidence in the reasoning and conclusions           |
| Forecast  | Estimated probability of occurrence                   |
| Knowledge | Strength of accumulated supporting evidence           |

**Information Flow Diagram**

```mermaid
flowchart TD

    %% ========= Reality =========
    A([Reality])

    %% ========= Sources =========
    A --> B[Information Sources]

    %% ========= Gate 1 =========
    B --> G1{"Gate 1<br/>Collection"}
    G1 -->|Accepted| C[Source]
    G1 -->|Rejected| X1[(Discard)]

    %% ========= Evidence =========
    C --> D[Evidence Collection]
    D --> E[Evidence]

    %% ========= Gate 2 =========
    E --> G2{"Gate 2<br/>Evidence Validation"}
    G2 -->|Valid| F[Validated Evidence]
    G2 -->|Invalid| X2[(Archive)]

    %% ========= Events =========
    F --> G3{"Gate 3<br/>Event Resolution"}

    G3 -->|Existing Event| H[Update Event]
    G3 -->|New Event| I[Create Event]

    H --> J[Event]
    I --> J

    %% ========= Entities =========
    J --> K[Entity Linking]
    K --> L[Entities]

    %% ========= Knowledge Base =========
    J --> KB
    L --> KB

    subgraph KB["Knowledge Base"]
        direction TB

        EL["Evidence Layer<br/>(Immutable observations)"]

        AL["Analytical Layer<br/>(Analysis Records<br/>Forecasts<br/>Claims)"]

        KL["Knowledge Layer<br/>(Validated patterns<br/>relationships<br/>lessons learned)"]

        EL --> AL
        AL --> KL
    end

    %% ========= Priority =========
    KB --> G4{"Gate 4<br/>Priority"}

    G4 -->|Low| X3[(Knowledge Only)]

    G4 -->|Medium / High| G5{"Gate 5<br/>Routing"}

    %% ========= Specialists =========
    G5 --> P[Political Analyst]
    G5 --> Q[Economic Analyst]
    G5 --> R[Military Analyst]
    G5 --> S[Science & Technology Analyst]
    G5 --> T[Governance Analyst]

    %% ========= Analysis =========
    P --> U[Analysis Record]
    Q --> U
    R --> U
    S --> U
    T --> U

    %% ========= Critique =========
    U --> G6{"Gate 6<br/>Claim Validation"}

    G6 --> V[Critic Agent]

    V --> W[Revised Analysis]

    %% ========= Human =========
    W --> G7{"Gate 7<br/>Human Review"}

    G7 -->|Reject| AL

    G7 -->|Approve| Y[Published Analysis]

    %% ========= Forecast =========
    Y --> Z[Forecast]

    Z --> AL

    %% ========= Time =========
    Z --> AA([Time Passes])

    AA --> AB([Reality Unfolds])

    %% ========= Learning =========
    AB --> G8{"Gate 8<br/>Evaluation"}

    G8 --> AC[Forecast Evaluation]

    AC --> AD[Knowledge Update]

    AD --> KL

```

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
SECTION 4: AGENT TAXONOMY AND REPOSITORIES
-------------------------------------------------------------------------------------------------------------------------------------------------------------------



-------------------------------------------------------------------------------------------------------------------------------------------------------------------
SECTION 5: TECHNOLOGY MAPPING
-------------------------------------------------------------------------------------------------------------------------------------------------------------------




