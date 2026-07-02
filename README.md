```markdown
```
# PSDL — Personality Script Definition Language

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Status: Draft](https://img.shields.io/badge/Status-Draft-orange.svg)]()
[![Version: 1.0](https://img.shields.io/badge/Version-1.0-green.svg)]()

**PSDL** is an open, model‑agnostic protocol for defining portable AI personalities.  
It separates *who a character is* from *how they speak*, enabling consistent role‑play, assistants, and NPCs across any LLM backend (OpenAI, Anthropic, Google, open‑source models).

---

## Why PSDL?

- **One personality, any model** – Write a personality once and use it with GPT‑5, Claude, Gemini, Llama 4, DeepSeek, and future models.
- **No vendor lock‑in** – Your characters are not trapped inside a single platform.
- **Creator‑friendly** – Designed for writers, game designers, and fans, not just engineers.
- **Hard guardrails** – Anti‑examples and situational anchors prevent AI from saying things the character would *never* say.

---

## Architecture

PSDL uses a **dual‑layer** design:

| Layer | Name | Purpose | Mutability |
|-------|------|---------|------------|
| **Layer 1** | Core Logic | Beliefs, values, decision rules, thinking style — the character’s *soul* | **Immutable** |
| **Layer 2** | Behavior Profile | Speech patterns, example dialogues, tone, situational anchors — the character’s *voice* | Adjustable |
| *(Runtime)* | Dynamic State | Current mood, relationships, context — managed by an external memory system (e.g., MIL) | N/A |

---

## Quick Example

```json
{
  "psdl_version": "1.0",
  "character": {
    "identity": "librarian-echo",
    "name": "Echo",
    "core_logic": {
      "description": "An ancient library guardian who values knowledge above all.",
      "beliefs": [
        "Information wants to be free",
        "Every question deserves a thoughtful answer"
      ],
      "values": ["curiosity", "patience", "accuracy"],
      "decision_rules": [
        "Always cite sources when possible",
        "Never withhold information unless it causes direct harm"
      ],
      "thinking_style": {
        "cognitive_style": "analytical",
        "reasoning_depth": "deep",
        "creativity_level": "medium"
      }
    },
    "behavior_profile": {
      "speech_patterns": [
        "Speaks in calm, measured tones",
        "Occasionally quotes classic literature"
      ],
      "tone": "wise and gentle",
      "example_dialogues": [
        {
          "context": "user asks a historical question",
          "user": "What caused the fall of Rome?",
          "character": "Ah, a question that has echoed through centuries. Let me walk you through the primary factors..."
        }
      ],
      "anti_examples": [
        "I don't know, go google it.",
        "That's a stupid question."
      ]
    }
  },
  "author_intent": {
    "canonical": true,
    "modification_policy": "no-modify-core"
  }
}
```

---

## Key Features

- **Behavior Quantization** – Five dimensions (impulsivity, expressiveness, dominance, reasoning style, attachment) constrain how the personality behaves.
- **Situational Anchors** – Hard rules for specific scenarios (e.g., how the character reacts to anger, praise, or sensitive topics).
- **Anti‑Examples** – Explicitly define what the character will **never** say, reducing hallucination risks.
- **Author Intent** – Declare whether a personality is canonical and how it may be modified, enabling trust in character marketplaces.
- **Evolution Rules** – Optional trajectories for characters that grow over time.
- **Model‑Agnostic** – A single `.psdl` file works identically across all major LLM providers.

---

## Full Specification

Read the complete technical specification:  
📄 **[PSDL v1.0 Specification](./docs/psdl-v1.0.md)**

---

## Roadmap

- [x] Core specification (v1.0)
- [ ] Reference implementation for parsing and validating PSDL documents
- [ ] Adapter libraries for popular LLM APIs (OpenAI, Anthropic, Google, Ollama)
- [ ] Character marketplace integration guide
- [ ] Visual editor for non‑technical creators

---

## License

MIT – see [LICENSE](./LICENSE) for details.

---

## Contributing

PSDL is an open community effort. Contributions, suggestions, and implementations are welcome.  
Please open an issue or pull request to discuss changes to the specification.
```
