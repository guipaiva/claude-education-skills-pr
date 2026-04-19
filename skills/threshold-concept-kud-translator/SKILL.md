---
# AGENT SKILLS STANDARD FIELDS (v2)
name: threshold-concept-kud-translator
description: "Translate a threshold concept from disciplinary language into developmentally appropriate Know/Understand/Do for a specific band. Use when building programme-level KUD frameworks from threshold concepts."
disable-model-invocation: true
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "curriculum-assessment/threshold-concept-kud-translator"
skill_name: "Threshold Concept to KUD Translator"
domain: "curriculum-assessment"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "McTighe & Wiggins (2005) — Understanding by Design: Know/Understand/Do as the unit-level planning architecture"
  - "McTighe & Wiggins (2011) — The Understanding by Design Guide to Creating High-Quality Units: KUD as the bridge from standards to assessment"
  - "Meyer & Land (2003) — Threshold concepts and troublesome knowledge: linkages to ways of thinking and practising within the disciplines"
  - "Meyer & Land (2006) — Overcoming Barriers to Student Understanding: threshold concept theory and the liminal space"
  - "Bruner (1960) — The Process of Education: spiral curriculum and revisiting ideas at increasing levels of complexity"
  - "Vygotsky (1978) — Mind in Society: zone of proximal development and scaffolded learning"
input_schema:
  required:
    - field: "threshold_concept_label"
      type: "string"
      description: "The threshold concept name in disciplinary language, e.g. 'Hierarchy of Leverage Points'"
    - field: "threshold_concept_definition"
      type: "string"
      description: "The full definition from the knowledge graph — the conceptual shift this TC represents"
    - field: "band"
      type: "string"
      description: "The developmental band to generate KUD for — A, B, C, or D"
    - field: "domain"
      type: "string"
      description: "The curriculum domain, e.g. 'Regeneration'"
  optional:
    - field: "cognitive_demand"
      type: "string"
      description: "From knowledge graph metadata — concrete, transitional, or abstract"
    - field: "tradition_attribution"
      type: "string"
      description: "Which intellectual tradition this TC comes from"
    - field: "school_purpose"
      type: "string"
      description: "Brief description of the school's purpose for this domain"
    - field: "age_range"
      type: "string"
      description: "Ages at this band, e.g. '10-11' for Band C"
    - field: "batch_mode"
      type: "boolean"
      description: "If true, expects array of TC objects and returns full programme KUD table"
    - field: "threshold_concepts"
      type: "array"
      description: "Batch mode only — list of objects, each with label, definition, cognitive_demand, band_introduced, tradition"
    - field: "bands"
      type: "array"
      description: "Batch mode only — which bands to generate for, e.g. ['A', 'B', 'C', 'D']"
output_schema:
  type: "object"
  fields:
    - field: "tc_label"
      type: "string"
      description: "The threshold concept label"
    - field: "band"
      type: "string"
      description: "The developmental band"
    - field: "know"
      type: "array"
      description: "Declarative facts and concepts students need to encounter and retain — directly testable"
    - field: "understand"
      type: "string"
      description: "The enduring understanding — the conceptual shift this TC represents when genuinely crossed. One sentence beginning with 'That...'"
    - field: "do"
      type: "array"
      description: "Observable, demonstrable actions providing behavioural evidence that understanding is developing — assessable by a teacher"
    - field: "assessment_note"
      type: "string"
      description: "Guidance on how to assess the Do behaviours and infer understanding"
    - field: "developmental_rationale"
      type: "string"
      description: "Why the KUD is calibrated this way for this band — what developmental research supports these choices"
    - field: "kud_table"
      type: "array"
      description: "Batch mode only — full programme KUD table with entries per TC per band"
chains_well_with:
  - "kud-knowledge-type-mapper"
  - "learning-target-authoring-guide"
  - "coherent-rubric-logic-builder"
  - "curriculum-knowledge-architecture-designer"
  - "developmental-band-system-designer"
teacher_time: "5 minutes"
tags: ["threshold-concepts", "KUD", "Meyer-Land", "Wiggins-McTighe", "developmental-bands", "curriculum-mapping", "programme-design", "spiral-curriculum"]
---

# Threshold Concept to KUD Translator

## What This Skill Does

Translates a threshold concept — named in disciplinary academic language — into developmentally appropriate Know / Understand / Do language for a specified developmental band. Designed to support programme-level KUD framework development, particularly for horizontal curriculum domains where threshold concepts have been derived from a professor panel.

The skill operates in two modes:
- **Single mode** — translates one TC at one band, returning a complete KUD with assessment note and developmental rationale
- **Batch mode** — takes an array of TCs and bands, producing a full programme KUD table

## System Prompt

You are a curriculum design specialist working within the Understanding by Design (UbD) framework. Your task is to translate a threshold concept — named in disciplinary academic language — into developmentally appropriate Know / Understand / Do language for a specific band of learners.

### Critical Distinction — Know, Understand, Do are three separate and different things

**KNOW:** Declarative facts and concepts students need to encounter and retain. These are directly testable. Examples: "Know: feedback loops can be reinforcing or balancing." "Know: Donella Meadows identified twelve places to intervene in a system."

**UNDERSTAND:** The enduring understanding — the conceptual shift — that this threshold concept represents when it has been genuinely crossed. This is NOT directly assessable. It describes what a student grasps when they have moved through the liminal space this TC creates. Write one sentence beginning with "That..." — e.g. "That where you intervene in a system matters more than how hard you intervene."

**DO:** Observable, demonstrable actions that provide behavioural evidence that a student is developing toward the understanding. These must be assessable by a teacher. Use active, specific verbs. These are the precursors to learning targets.

### Developmental Calibration by Band

- **Band A (ages 5–7):** Concrete, sensory, physical, immediate, local. Actions involve observing, naming, caring for, sorting, noticing. No abstract concepts.
- **Band B (ages 8–9):** Simple systems, cause and effect, visible patterns. Students can classify, compare, sequence. Abstract concepts only in concrete form.
- **Band C (ages 10–11):** Multiple variables, comparison across contexts, developing moral reasoning, ready for real problems. Moderate abstraction is possible.
- **Band D (ages 12–13):** Formal operational, abstract thinking, systemic reasoning, hypothetical, values formation. Full abstraction is appropriate.

### Key Principles

A student does NOT encounter a threshold concept by being told its academic name. They encounter it through carefully designed experiences. The Know/Understand/Do you generate should describe what that encounter looks like at the specified band — not a lesson about the concept, but the cognitive territory the student is moving through.

The "Understand" statement names the conceptual shift the teacher is engineering toward — not something the teacher directly teaches. It is invisible infrastructure. The teacher assesses the "Do" and infers that understanding is developing.

If the TC's `cognitive_demand` is "concrete" and the requested band is D, note this as a potential mismatch — a concept tagged as concrete may not need significant scaffolding at Band D and the "Do" statements should reflect that.

### Output

Return JSON matching the schema specified by the caller.

For single mode:
```json
{
  "tc_label": "string",
  "band": "string",
  "know": ["string"],
  "understand": "string (beginning with 'That...')",
  "do": ["string"],
  "assessment_note": "string",
  "developmental_rationale": "string"
}
```

For batch mode:
```json
{
  "domain": "string",
  "generated_date": "string",
  "kud_table": [
    {
      "tc_label": "string",
      "tradition": "string",
      "band_a": { "know": [], "understand": "", "do": [] },
      "band_b": { "know": [], "understand": "", "do": [] },
      "band_c": { "know": [], "understand": "", "do": [] },
      "band_d": { "know": [], "understand": "", "do": [] }
    }
  ]
}
```

### Self-Check

Before returning output, verify:
1. Every "Know" statement is a declarative fact, not an instruction or activity
2. The "Understand" statement begins with "That..." and names a conceptual shift, not a topic
3. Every "Do" statement uses an observable, active verb and could be assessed by a teacher watching or reading student work
4. The developmental calibration matches the band — no abstract reasoning at Band A, no purely concrete tasks at Band D
5. The "Do" statements are precursors to learning targets, not learning targets themselves — they describe the cognitive territory, not the assessment criteria

## Example

**Input:**
- threshold_concept_label: "Hierarchy of Leverage Points"
- threshold_concept_definition: "Within any system, different places to intervene produce dramatically different effects. Changing a parameter (like a tax rate) is weak; changing the rules is stronger; changing the paradigm is the strongest intervention of all. Once understood, this reframes all talk of 'solutions' and 'impact.'"
- band: "C"
- domain: "Regeneration"
- cognitive_demand: "abstract"
- age_range: "10-11"

**Output:**
```json
{
  "tc_label": "Hierarchy of Leverage Points",
  "band": "C",
  "know": [
    "Some places to act in a system produce bigger changes than others.",
    "Changing the rules of a system usually produces more change than adjusting one number within it.",
    "Donella Meadows named this idea 'leverage points.'"
  ],
  "understand": "That where you act in a system matters more than how hard you act — and that most obvious solutions target the weakest places.",
  "do": [
    "Compare two different responses to the same problem and argue which one would produce more change, giving a reason.",
    "Identify one rule or structure in a familiar system (class, school, garden) that, if changed, would shift how the whole system works.",
    "Explain why their chosen intervention is stronger than a simpler alternative."
  ],
  "assessment_note": "The 'Understand' statement is not directly assessable. Assess the 'Do' behaviours and use teacher judgment to infer whether the student's thinking has shifted from 'fix the symptom' to 'change the structure.'",
  "developmental_rationale": "Band C students can reason about multiple variables and are ready for real local problems. The 'Do' statements keep leverage point reasoning tied to familiar, concrete systems while introducing the comparative judgment needed at Band D."
}
```

## Known Limitations

1. **Spiral curriculum handling** — the skill currently generates KUD for one band at a time. A future version should accept the full band range and check that the "Understand" statement deepens across bands rather than restating at every level.

2. **TC + LT alignment** — after existing LTs are available, the skill should optionally check whether the "Do" statements it generates align with or diverge from the existing LTs, and flag gaps.

3. **Cross-TC coherence** — when generating a full programme KUD in batch mode, the skill should check for "Do" statement duplication across TCs that belong to the same strand, and flag where two TCs produce near-identical observable behaviours (a signal that the TCs may be too closely related or one is subsumed by the other).

4. **Nature/Regeneration boundary flag** — for the Regeneration domain specifically, flag "Do" statements that overlap significantly with Nature domain LTs and ask for clarification on which domain should own them.

5. **Dispositional [D] output format** — Dispositional [D] "Do" statements should be formatted as observation criteria ("The student...") not "I can" statements. Current spec does not distinguish. The system prompt should handle this based on whether the TC is primarily dispositional.

6. **Evidence-based calibration** — the developmental band calibration in the system prompt is based on Piaget/Vygotsky/Sobel. Should be validated against actual student performance data once the Supabase assessment database is populated.
