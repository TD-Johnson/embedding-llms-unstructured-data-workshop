# Episode map (locked structure v2)

### Format and delivery
- 00 and 06: Powerpoint slides with instructor notes in episodes/
- 01-05: Jupyter notebooks in notebooks/
- Participants download the repo as a ZIP from GitHub and run the notebooks
  locally in VS Code, in a uv environment (`uv sync`) — set up before the day
  using learners/00-setup.md
- LLM runs on Dell Pro Max (GB10) machines serving vLLM inside the university
  network — participants must be on the university VPN
- Each participant gets a base URL + API key on the day and stores them in a
  .env file (notebook 01)

### Designated cut on the day
Episode 04 is the cut episode if running behind.
Participants lose a fun exercise, not a core concept.

### Narrative spine (paper reference throughout)
Every episode connects to Ardekani et al. (2026):
"Computational and Graph-Theoretic Analysis of Legislative Networks:
NZ Mental Health Act as Case Study" — MDPI Information, Feb 2026
https://www.mdpi.com/2078-2489/17/2/161

Episode → Paper component mapping:
01 → Ingestion (Step 1): XML parsing infrastructure
02 → Stochasticity finding: 86% vs 96% precision, temperature effects
03 → Exploratory analysis + Extraction (Step 2): Tai et al. presence/absence coding
04 → Cross-modal application: same pipeline, image data;
      Jaccard validation against committee domains (Step 6)
05 → Extraction at scale + Semantic Enrichment + Validation
      (Steps 2 + 4 + 5): loop over many documents, check against an
      independent reference

### Primary dataset
Privacy Act 2020 (used in NB01–NB03)
https://legislation.govt.nz/act/public/2020/31/en/latest.xml

### Fun Act for NB02 implicit knowledge exercise
Wool Board Disestablishment Act 2009
https://legislation.govt.nz/act/public/2009/62/en/latest.xml
Purpose: demonstrate that LLM implicit knowledge is patchy on obscure Acts
vs confident on well-known ones like the Privacy Act

### Episode map

00 — Workshop setup (15 mins)
     Format: Powerpoint + instructor notes in episodes/00-setup.md
     Key message: "you can, not you must"
     Paper intro: show the six-step workflow on a slide as the day's map
     Caveats: not a qualitative researcher; real methods real data;
              ethics and supervisor conversations still required
     Outcomes: expectations set, positionality established

01 — Environment setup (20 mins)
     Notebook: notebooks/01-environment-setup.ipynb
     Paper component: Step 1 — Ingestion infrastructure
     Core concept: VS Code notebooks, Python basics, vLLM API, XML parsing
     Exercises:
     - Python cells as calculator
     - Variables and print()
     - Base URL + API key stored in .env (rename .env.example)
     - Run setup cell → "Setup complete." The setup cell contacts the
       Dell Pro Max and looks up its model (client.models.list()), so
       it confirms VPN and API key.
     - See which model you are using: print everything the machine serves
       next to MODEL. No chat call — participants do not talk to the LLM
       until NB02.
     Framing sentence: "Step 1 of the paper's workflow is ingestion —
     parsing the XML files NZ government publishes for every Act.
     By the end of this notebook you will have exactly that."
     Outcomes: environment working, API connected, no fear of errors

02 — LLMs as research instruments (20 mins)
     Notebook: notebooks/02-llms-as-instruments.ipynb
     Paper component: Stochasticity finding (86% vs 96% precision)
     Core concept: LLMs as probabilistic instruments, not oracles
     Exercises:
     a. First message to the model (moved here from NB01) — send
        "What is the capital of France?" — model + messages only, no
        extra_body. Then walk through client.chat.completions.create():
        model, messages, role/content,
        response.choices[0].message.content. "Your turn" cell adds
        extra_body=NO_THINKING (explained there) and asks a question
        from their own research area.
     b. Ask model about Privacy Act 2020 (no source data)
        — what does it know implicitly?
     c. Ask model about Wool Board Disestablishment Act 2009
        — contrast: knowledge is patchy on obscure Acts
     d. Temperature comparison: same prompt at temp=0 (twice)
        and temp=1 (twice)
        Discussion: "The paper measured LLM precision at 86% vs 96%
        for rule-based methods. You are now experiencing why."
     e. Introduce prompt structure: role / instruction / context /
        output format
     Framing sentence: "The paper found LLMs are less precise and
     less consistent than deterministic methods — temperature is
     exactly why. What you observe now is what the authors measured."
     Outcomes: understand temperature and stochasticity; mental model
               of LLM as instrument; basic prompt structure

---- BREAK (10 mins) ----

03 — Exploratory analysis and precision coding (30 mins)
     Notebook: notebooks/03-prompt-engineering.ipynb
     Paper components: Component 2 — Extraction Dilemma
                       (Ardekani et al. 2026); Tai et al. (2024)
                       presence/absence coding method
     Dataset: Privacy Act 2020 — Information Privacy Principle 3
              passage embedded directly in the notebook (no live fetch)

     Act 1 [mandatory, ~15 mins]:
     Exploratory analysis progression
     - Build the same request in five progressive versions, adding
       one layer each time, all run against the same passage:
       v1 — bare instruction
       v2 — add word limit (<20 words)
       v3 — add role ("You are a policy analyst")
       v4 — add output format constraints (plain English, no jargon,
            no preamble)
       v5 — add perspective ("privacy rights advocate concerned with
            surveillance risks") — fill-in-the-blanks cell
     - Store outputs in a dictionary; print a comparison table with
       automatic word counts via len(output.split())
     - 20-word constraint is measurable so participants can verify
     Discussion: "Which version would you cite in a methods section?
                 Which gives the most consistent results on re-run?
                 Why does v5 shift emphasis even with identical
                 formatting constraints?"
     Framing sentence: "The paper found LLM extraction precision of
     86% vs 96% rule-based. One reason for that gap is prompt design.
     The same question, asked differently, gives meaningfully
     different results."

     Act 2 [mandatory, ~10 mins]:
     Tai et al. (2024) replication — presence/absence coding
     Eight keywords coded for presence (1) or absence (0):
     consent / individual / purpose / disclosure / collect /
     payment / children / employment (last three are absent —
     a check that coders, human and LLM, can say "no")
     - Step 1: Manual coding — participants fill `your_coding` dict
       with 1/0 values for each keyword BEFORE running LLM cell
     - Step 2: LLM coding — prompt returns JSON only; parsed into
       `llm_coding` dict with try/except around json.loads
     - Step 3: Comparison table — auto-generated from the two dicts,
       counts agreements, prints "X out of 8"
     Reference framing: "Tai et al. (2024) used an LLM to code
     presence/absence of psychological constructs in interview
     transcripts. Agreement with human coders was comparable to
     inter-rater reliability between two humans. We replicate that
     logic on NZ legislation."

     Act 3 [mandatory, ~5 mins]:
     Disagreement resolution
     - Auto-identify disagreements between your_coding and llm_coding
     - If no disagreements: congratulate + prompt participant to flip
       one coding manually to test the resolution loop
     - For each disagreement: display both codings, send follow-up
       prompt asking LLM to explain its reasoning in 2-3 sentences
       anchored to specific parts of the text
     - Prompt participant: "Do you agree? Note your answer — this is
       your spot-check record."
     Reflection: disagreement is information, not failure. Either the
     LLM surfaced something you missed, or your check caught an error.
     Tai et al. reported ~86% agreement — how does today's rate
     compare? What changes at 500 sections vs 1?

     Test yourself [~5 mins, cut Q1 code if behind]:
     Two questions before "What you accomplished". Answers are not in
     the learner notebook — go through them as a group.
     - Q1 — Fix a weak prompt. Learners rewrite
       "Tell me about this. Keep it short." for <25 words, plain
       English, no preamble, community law centre volunteer audience.
       Answer (a): all four layers are missing. "Keep it short" is not
       a measurable word limit. Answer (b): accept any version that
       runs and comes in under 25 words. A good answer has a system
       message with a role/perspective, plus a user message with a
       numeric limit, "plain English", and "return only the summary,
       no preamble".
     - Q2 — A convincing explanation (multiple choice). The LLM
       justifies consent=1 by quoting "express written consent",
       which is not in the passage.
       Answer: C. The quote is a hallucination; the checking cell
       prints False. Point out that consent is still arguably present
       via IPP 11(c) "authorised by the individual concerned", so 1
       may be the right code — but you get there by checking the
       source, not by trusting a confident explanation (A) or
       dismissing the LLM out of habit (B). D (re-running until it
       agrees) is cherry-picking, which is a validity problem in
       itself.

     Bridge to NB04: "You controlled output through prompt structure
     and tested coding reliability against your own judgement. NB04
     uses the same skills — structured prompts and checking against
     a trusted source — on a new kind of data: images."

     Outcomes: prompt structure as a methodological choice;
               presence/absence coding replicated from Tai et al.;
               disagreement as evidence; bridge to image extraction

04 — Visual feature extraction (25 mins) [DESIGNATED CUT IF BEHIND]
     Notebook: notebooks/04-visual-extraction.ipynb
     Paper component: no direct equivalent — demonstrates pipeline
                      generalises beyond legislative text
     Dataset: Archives NZ via Wikimedia Commons API
              Curated corpus (confirmed working):
              - Prosecution_of_Strikers-_The_1913_Black_List_(29788240653).jpg
              - 1870_Petition_of_Unemployment_(9926603304).jpg
              - 1879_Petition_against_Steam_Trams_(29078906095).jpg
     
     Setup [pre-written, ~3 mins]:
     All infrastructure cells pre-written — participant runs them,
     image loads and displays as payoff moment
     
     Exercise a [mandatory, ~8 mins]:
     Structured JSON extraction from image
     Fill-in-the-blanks prompt options provided:
     - scene description
     - political claim
     - visible text
     - symbols
     - keywords (10-15) ← feeds into exercise b
     - confidence level
     
     Exercise b [mandatory, ~10 mins]:
     Cross-modal Jaccard validation
     - Pre-written jaccard_similarity() function; Jaccard similarity
       is introduced here for the first time
     - Compare image keywords against COMMITTEE_KEYWORDS
     - Output: "This image most aligns with [X] committee"
     - Discussion: "Does the model's assessment match what you see?
       What does a discrepancy tell you?"
     - Framing: "You used the same extraction and validation pipeline
       on an image that you used on text. The data type changed.
       The methodology did not."
     
     Optional exercises [take-home, linked from GitHub Pages]:
     - Try a different image from WORKSHOP_IMAGE_CORPUS
     - Run with a positioned prompt, compare Jaccard scores
     - Try adversarial prompting on an image description
     
     Outcomes: consolidate validation techniques across data types;
               see pipeline generalises beyond text

---- BREAK (10 mins) ----

05 — Looping an LLM over many documents (40 mins)
     Notebook: notebooks/05-looping-over-documents.ipynb
     Paper component: Steps 2 + 4 + 5 — extraction at scale, semantic
                      enrichment (per-document summaries), analysis
     Dataset: 306 Givealittle health fundraising campaigns, scraped
              ahead of time; CSV downloaded from the workshop's GitHub
              repository (data/givealittle_health.csv)
     Builds on: NB03 (JSON output) and NB04 (image preparation,
                text + image content in one message)

     Part 1: What is a loop? — loop-and-collect pattern on three
             short interview snippets
     Part 2: The data — how it was collected (scraping etiquette),
             load the CSV, look at one campaign first
     Part 3: The extraction loop — text + hero photo per campaign,
             capped by MAX_CAMPAIGNS on a random sample; location held
             back on purpose for Part 4
     Part 4: Checking the LLM's work
             - Check 1: spot check one campaign yourself
             - Check 2: compare the model's region against the
               held-back location (independent reference)
             - Check 3: fields that nobody could verify
     Optional stretch: reuse the pattern on your own documents

     Outcomes: loop → gather → check pattern; a loop multiplies
               whatever the prompt does, so check before scaling up

06 — Wrap up (10 mins)
     Format: Powerpoint + discussion
     Return to six-step workflow slide from episode 00
     Walk through: participants did every step today
     Validation recap: consistency check / Jaccard / adversarial /
                       human spot-check — all citable methods
     Where next: search your domain, ethics application,
                 UoA GPU cluster Q2-Q3, paid APIs, Python fundamentals
     Final message: "You have seen what is possible. The path forward
                    is yours to choose."

### Timing
00: 15 + 01: 20 + 02: 20 + break: 10 + 03: 30 + 04: 25
+ break: 10 + 05: 40 + 06: 10 = 180 mins
No buffer left in 3 hours — cut 04 if behind (see above)

### COMMITTEE_KEYWORDS (fixed asset — use in NB04)
```python
COMMITTEE_KEYWORDS = {
    "Justice Committee": [
        "criminal", "justice", "court", "offence", "penalty",
        "prosecution", "enforcement", "police", "imprisonment",
        "tribunal", "legal", "rights", "appeal", "sentence", "conviction"
    ],
    "Health Committee": [
        "health", "medical", "treatment", "patient", "clinical",
        "disease", "mental", "disability", "care", "hospital",
        "practitioner", "pharmaceutical", "wellbeing", "public health",
        "safety"
    ],
    "Environment Committee": [
        "environment", "resource", "land", "water", "conservation",
        "climate", "emissions", "biodiversity", "sustainable",
        "pollution", "ecological", "planning", "consent", "impact",
        "natural"
    ],
    "Finance and Expenditure Committee": [
        "financial", "revenue", "tax", "expenditure", "budget",
        "fiscal", "economic", "payment", "fund", "investment",
        "commercial", "income", "cost", "penalty", "levy"
    ],
    "Governance and Administration Committee": [
        "privacy", "information", "data", "personal", "agency",
        "public", "government", "official", "disclosure", "access",
        "transparency", "accountability", "complaint", "commissioner",
        "record"
    ],
    "Social Services and Community Committee": [
        "social", "community", "welfare", "family", "housing",
        "poverty", "employment", "education", "youth", "elderly",
        "disability", "support", "benefit", "vulnerable", "protection"
    ]
}
```

### Known risk points
| Risk | Mitigation |
|------|-----------|
| Setup not done before the day | Walk through learners/00-setup.md on projector during 00; pair with a neighbour |
| Setup cell: "Could not connect to the LLM" | Check VPN first, then the .env values |
| Setup cell: "Could not find LLM_BASE_URL" | File still named .env.example, saved in notebooks/, or not saved |
| ModuleNotFoundError | Wrong kernel — select .venv in the top-right kernel picker |
| Slow responses | Several participants share each machine; spread participants across machines |
| Givealittle data download fails | NB05 downloads data/givealittle_health.csv from GitHub; check internet access |
| NB04 over time | Designated cut — skip cleanly |
| Participants stuck | Encourage skipping and continuing — errors expected |
| Jaccard function errors | Pre-test function in NB04 before delivery |
