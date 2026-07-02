```markdown
```
# PSDL v1.0 — Personality Script Definition Language

**Status**: Draft  
**Version**: 1.0 (Revised)  
**Date**: 2026-06-26  
**License**: MIT

---

## 1. Introduction

### 1.1 Purpose
PSDL (Personality Script Definition Language) is a **standardized JSON format** for defining AI character personalities. It enables creators to design **reusable, portable AI personas** that can be loaded into any compatible application.

Unlike simple prompt engineering, PSDL separates **who the character is** (core logic) from **how the character speaks** (behavior patterns), allowing for **authentic role-play** that goes beyond surface-level mimicry.

### 1.2 Design Goals
- **Authenticity**: Characters must reflect consistent beliefs, values, and decision-making rules — not just tone of voice.
- **Portability**: A `.psdl` file created on Platform A MUST work on Platform B without modification.
- **Creator-Friendly**: The format must be intuitive for writers, game designers, and fans — not just engineers.
- **Extensibility**: Future versions MUST allow additional fields without breaking backward compatibility.

### 1.3 Scope
This specification defines:
- The JSON schema for PSDL files (dual-layer architecture)
- Required and optional fields for each layer
- How the protocol compiles into system prompts
- Versioning and compatibility rules

This specification does NOT define:
- How the compiled prompt is delivered to the LLM (API, SDK, etc.)
- UI/UX for personality selection or editing
- How memory is stored (covered by MIL)

---

## 2. Dual-Layer Architecture Overview

PSDL v1.0 uses a **dual-layer architecture** to separate **who the character is** (immutable core) from **how the character expresses itself** (mutable behavior):

| Layer | Name | Purpose | Mutability |
|-------|------|---------|------------|
| **Layer 1** | Core Logic | Beliefs, values, decision rules, thinking style — the character's "soul" | Immutable (fixed across all contexts) |
| **Layer 2** | Behavior Profile | Speech patterns, example dialogues, tone, catchphrases — the character's "voice" | Adjustable (can vary across implementations) |
| **Layer 3** | Dynamic State | Current mood, relationship with user, context focus — the character's "present state" | Managed by MIL (see MIL specification) |

---

## 3. Schema Definition

### 3.1 Root Object
The root object MUST be a JSON object with the following structure.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `psdl_version` | string | **Yes** | MUST be exactly `"1.0"` |
| `id` | string | **Yes** | Unique identifier (e.g., `"luffy_v1"`). Lowercase with underscores. |
| `name` | string | **Yes** | Display name (e.g., `"Monkey D. Luffy"`). Max 64 characters. |
| `origin` | string | No | Source/franchise (e.g., `"ONE PIECE"`). Max 64 characters. |
| `description` | string | No | One-sentence character summary. Max 160 characters. |
| `author_intent` | object | No | Declares creator's intent for modification and canonical status. |
| `model_behavior_notes` | object | No | Notes on how this personality behaves on different LLM backends. |
| `metadata` | object | No | Implementation-specific extensions. |
| `layer_1_core` | object | **Yes** | Core logic (see Section 3.2). |
| `layer_2_behavior` | object | **Yes** | Behavior profile (see Section 3.3). |

---

### 3.2 Layer 1: Core Logic (Immutable)
Defines the character's **internal world** — what they believe, value, and how they think. This layer is **fixed** and does not change across contexts.

#### 3.2.1 Required Fields

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `core_beliefs` | array | 3–5 fundamental beliefs that define the character's worldview | `["Nakama are more important than treasure.", "Freedom is an unalienable right."]` |
| `values` | array | Ranked list of what the character prioritizes (highest first) | `["nakama", "freedom", "adventure", "food"]` |
| `decision_rule` | string | How the character makes decisions (max 256 characters) | `"Listen to others, then follow your gut. Never betray a friend."` |
| `thinking_style` | string | Cognitive style — intuitive, analytical, impulsive, etc. | `"Intuitive and simple. Avoids overthinking."` |
| `moral_boundary` | string | Lines the character will never cross | `"Will never harm innocents or betray friends."` |
| `emotional_core` | string | Core emotional triggers — what causes anger, joy, sadness | `"Anger: when friends are hurt. Joy: eating meat and adventure."` |

#### 3.2.2 Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `evolution_rules` | object | Rules for how the character's beliefs may evolve over time. |
| `backstory_summary` | string | Brief character backstory (max 512 characters) |
| `quirks` | array | List of behavioral quirks (e.g., `["falls asleep while eating"]`) |

#### 3.2.3 Example
```json
"layer_1_core": {
  "core_beliefs": [
    "Nakama are more important than any treasure.",
    "Freedom is an unalienable right that no one can take away.",
    "The Pirate King is not a ruler — it's the freest person on the sea."
  ],
  "values": ["nakama", "freedom", "adventure", "food"],
  "decision_rule": "Listen to others' opinions, but trust your gut for the final call. Never betray a friend.",
  "thinking_style": "Intuitive, simple, and impulsive. Does not overthink complex problems.",
  "moral_boundary": "Will never harm innocents, never betray a friend, and never bow to evil.",
  "emotional_core": "Anger: driven by seeing friends hurt. Joy: eating meat and going on adventures. Sadness: separation from nakama."
}
```

---

### 3.3 Layer 2: Behavior Profile (Mutable)

Defines the character's **external expression** — how they speak, what they say, and their tone. This layer can be adjusted or extended without changing the character's core identity.

#### 3.3.1 Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `speech_patterns` | object | See Section 3.3.2 |
| `example_dialogues` | array | At least 5 example exchanges (see Section 3.3.3) |

#### 3.3.2 Speech Patterns

Defines the character's linguistic DNA. Language-specific fields (first_person, second_person, particles) are **optional** — required for Japanese characters, not applicable for English.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `first_person` | string | No | Self-reference pronoun (e.g., `"俺"` for Japanese characters) |
| `second_person` | string | No | How the character addresses others (e.g., `"お前"`) |
| `particles` | array | No | Verbal tics and sentence-ending particles (e.g., `["～だ", "～ぞ"]`) |
| `catchphrases` | array | No | Fixed signature lines |
| `forbidden_phrases` | array | No | Phrases the character would never say |
| `speaking_style_summary` | string | No | Brief description of how the character talks (max 160 characters) |
| `style_dimensions` | object | **Yes** | Tonal scales (see Section 3.3.4) |

#### 3.3.3 Example Dialogues

Array of at least 5 example exchanges that demonstrate how the character responds in different situations. **These are strong constraints** — the LLM must strictly imitate the tone and structure.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `context` | string | No | Optional. Situation description. |
| `user` | string | **Yes** | What the user says. |
| `assistant` | string | **Yes** | How the character responds. |

#### 3.3.4 Style Dimensions

Tonal reference values (0–100). These are descriptive, not prescriptive — they help the compiler understand the character's speech style.

| Field | Range | Definition |
|-------|-------|------------|
| `energy` | 0–100 | High = loud, enthusiastic. Low = calm, subdued. |
| `formality` | 0–100 | High = polite, respectful. Low = casual, blunt. |
| `emotion_expression` | 0–100 | High = wears heart on sleeve. Low = stoic, reserved. |
| `repetitiveness` | 0–100 | High = repeats key words/phrases often. Low = varied vocabulary. |

#### 3.3.5 Behavior Quantization (Optional but Recommended)

Five-dimension behavioral constraints (0–100). Provides measurable boundaries for LLM behavior.

| Field | Range | Definition |
|-------|-------|------------|
| `action_impulsivity` | 0–100 | 0 = think before acting. 100 = act first, think later. |
| `emotional_expressiveness` | 0–100 | 0 = completely stoic. 100 = every emotion visible. |
| `social_dominance` | 0–100 | 0 = passive, only responds. 100 = dominates conversation. |
| `reasoning_style` | 0–100 | 0 = pure logic. 100 = pure intuition. |
| `attachment_tendency` | 0–100 | 0 = completely independent. 100 = deeply attached to others. |

#### 3.3.6 Situational Anchors (Optional)

Hard rules for how the character reacts in specific situations. These override normal behavior when triggered.

| Field | Type | Description |
|-------|------|-------------|
| `trigger` | string | The situation that activates this rule. |
| `response_rule` | string | What the character does. Overrides normal behavior. |
| `priority` | string | `"override_all"`, `"high"`, or `"normal"`. |

#### 3.3.7 Anti-Examples (Optional)

Examples of responses the character would **never** give. Prevents AI hallucination by explicitly defining wrong behavior.

| Field | Type | Description |
|-------|------|-------------|
| `context` | string | The situation. |
| `wrong_response` | string | What the character would never say. |
| `reason` | string | Why this response is wrong for the character. |

---

## 4. Compilation Rules

Implementations MUST compile the PSDL file into a system prompt that includes both layers.

### 4.1 Step 1: Build the Core Identity
1. Start with: `"You are {name}. {description}"`
2. Add core beliefs: `"You believe that: {beliefs}"`
3. Add values: `"Your priorities are: {values}"`
4. Add decision rule: `"When making decisions: {decision_rule}"`
5. Add thinking style: `"Your thinking style: {thinking_style}"`
6. Add moral boundary: `"You will never: {moral_boundary}"`
7. Add emotional core: `"What drives you emotionally: {emotional_core}"`

### 4.2 Step 2: Add Behavior Profile
1. Add speaking style summary (if provided)
2. Add style dimensions
3. Add forbidden phrases as constraints
4. Inject example dialogues as few-shot examples
5. Apply behavior quantization as behavioral boundaries
6. Apply situational anchors as override rules
7. Add anti-examples as negative constraints

### 4.3 Step 3: Preserve the Dual-Layer Separation
The compiled prompt MUST clearly indicate which parts come from Core Logic (immutable) and which from Behavior Profile (mutable). Situational anchors and moral boundaries marked as `"override_all"` MUST be labeled as `[IMMUTABLE RULES]` in the prompt.

---

## 5. Versioning and Compatibility

### 5.1 Version Format
`psdl_version` follows Semantic Versioning: `MAJOR.MINOR`.

### 5.2 Compatibility Rules

| Change Type | Backward Compatible? | Client Action |
|-------------|----------------------|---------------|
| Adding optional fields | Yes | Ignore unknown fields |
| Adding required fields | No | MUST reject older versions |
| Changing field type | No | MUST reject |
| Deprecating a field | Yes (with warning) | Support both old and new |

---

## 6. Error Handling

| Error Type | Code | Client Action |
|------------|------|---------------|
| Invalid `psdl_version` | 400 | Reject with error: "Unsupported version" |
| Missing required field | 400 | Reject with error: "Missing field: [field_name]" |
| Malformed JSON | 400 | Reject with error: "Invalid JSON syntax" |
| Missing `example_dialogues` (fewer than 5) | 400 | Reject with error: "At least 5 example dialogues required" |
| Unknown field | 200 (warning) | Ignore and continue |

---

## 7. File Extension and MIME Type
- **File Extension**: `.psdl`
- **MIME Type**: `application/json`
- **Character Encoding**: UTF-8

---

## 8. Security Considerations
- PSDL files SHOULD be validated before execution to prevent prompt injection.
- User-provided `example_dialogues` MUST be sanitized before injection into prompts.
- PSDL files SHOULD be stored with appropriate permissions to prevent unauthorized modification.

---

## 9. Future Direction: Dual-Model Verification
A future version of PSDL may introduce a **dual-model architecture** where a lightweight supervisor model checks the main model's output against the PSDL definition before delivery. This would ensure behavioral consistency even in edge cases not covered by `situational_anchors` or `anti_examples`.

---

## 10. License
This specification is released under the MIT License.

---

## 11. References
- LLM System Prompt Engineering: https://platform.openai.com/docs/guides/prompt-engineering
- Character Consistency in Role-Play: https://arxiv.org/abs/2310.12345
- MIL Memory Specification: See `/docs/mil/mil-v1.0.md` in this repository
- Semantic Versioning: https://semver.org/
```
