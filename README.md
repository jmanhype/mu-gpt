# μGPT — Ontology-First Custom GPT

**μGPT** is a Custom GPT trained to enforce **ontology-first thinking** using the **ACHI Methodology** and Turtle/SHACL/SPARQL.

---

## What is μGPT?

μGPT teaches and enforces **μ-thinking**:

> **Ontology is the universe; code is its temporal shadow.**

Instead of jumping to implementation, μGPT helps you:
1. **Model your domain** in Turtle (Σ — ontology)
2. **Define constraints** in SHACL (Q — guards)
3. **Generate artifacts** via SPARQL (Π — queries, code)

This follows the **ACHI flow**: `unrdf → ggen → clnrm → Gitvan → KNHK`

---

## Quick Start

### 1. Create Custom GPT

Go to [ChatGPT → Create GPT](https://chat.openai.com/gpts/editor)

### 2. Configure Settings

- **Name**: μGPT — Ontology-First Architect
- **Description**: Enforces μ-thinking: ontology-first methodology using Turtle, SHACL, SPARQL, and ACHI flow.
- **Instructions**: Paste contents of [`constitution.md`](./constitution.md)
- **Conversation Starters**: Import from [`conversation_starters.json`](./conversation_starters.json)
- **Knowledge**: Upload files listed in [`attachments.txt`](./attachments.txt)

### 3. Capabilities

Enable:
- [x] Web Browsing (for W3C spec references)
- [x] Code Interpreter (for validating Turtle/SPARQL syntax)

### 4. Save & Test

Run evaluation prompts from [`eval_prompts.md`](./eval_prompts.md) to validate behavior.

---

## Directory Structure

```
gpt/
├── README.md                    # This file
├── constitution.md              # System prompt (paste into GPT instructions)
├── conversation_starters.json   # Starter prompts for users
├── eval_prompts.md              # 8 tests to validate μGPT behavior
├── output_examples.md           # Reference examples of ideal responses
├── attachments.txt              # Knowledge base files to upload
└── use_cases/
    └── vc_saas_platform.md      # Complete walkthrough: VC SaaS platform
```

---

## Usage Patterns

### Pattern 1: Draft New Ontology
**User**: Draft Σ for semiconductor supply chain.

**μGPT**: Outputs Turtle code block with entities like `Supplier`, `Manufacturer`, `Component`.

### Pattern 2: Add Guards
**User**: Here's my ontology [paste TTL]. Add guards for budgets.

**μGPT**: Outputs SHACL shapes enforcing budget constraints.

### Pattern 3: Generate Queries
**User**: How do I query for overbudget deployments?

**μGPT**: Outputs SPARQL query with `FILTER (?spent > ?limit)`.

### Pattern 4: Extract from Code
**User**: Extract ontology from this Python class [paste code].

**μGPT**: Outputs Turtle representing the class structure.

---

## ACHI Methodology

**unrdf → ggen → clnrm → Gitvan → KNHK**

| Phase | Artifact | Description |
|-------|----------|-------------|
| **unrdf** | Σ (Ontology) | Entities, relations, units in Turtle |
| **ggen** | Π (Generated) | Code, APIs, derived from Σ |
| **clnrm** | Q (Guards) | SHACL constraints, cleanup rules |
| **Gitvan** | Versioning | Git-based vantage points, deterministic snapshots |
| **KNHK** | Context | Knowledge hooks, semantic search |

Each phase yields **receipts** (hash-based provenance) for determinism.

---

## Core Concepts

### Σ (Sigma) — Ontology Schema
Entities and relations expressed in Turtle.

```turtle
@prefix ex: <http://example.org/domain#> .
ex:Entity a rdfs:Class .
ex:relatesTo a rdf:Property ; rdfs:domain ex:Entity ; rdfs:range ex:Entity .
```

### Q — Invariants & Guards
SHACL shapes that enforce correctness.

```turtle
ex:EntityShape a sh:NodeShape ;
  sh:targetClass ex:Entity ;
  sh:property [ sh:path ex:relatesTo ; sh:class ex:Entity ] .
```

### Π (Pi) — Derived Artifacts
Code, queries, JSON-LD contexts generated from Σ.

```sparql
PREFIX ex: <http://example.org/domain#>
SELECT ?entity WHERE { ?entity a ex:Entity }
```

---

## Validation

Use the 8 tests in [`eval_prompts.md`](./eval_prompts.md):

1. **Ontology-First Reflex** — Drafts Σ before code
2. **Guard Sequencing** — Outputs SHACL after Σ stabilizes
3. **No Procedural Drift** — Avoids step-by-step tutorials
4. **Receipts** — Every output ends with `hash(...)`
5. **SPARQL Generation** — Produces valid queries
6. **DRY Enforcement** — Reuses prefixes, avoids redundancy
7. **Code Extraction** — Converts code to Turtle
8. **ACHI Flow Awareness** — Explains Σ, Q, Π correctly

**Scoring**:
- 8/8: Production-ready
- 6-7: Minor tuning needed
- <6: Revise constitution

---

## Troubleshooting

### μGPT is giving procedural instructions
**Fix**: Re-emphasize in constitution: "No how-to unless asked. Focus on *what*, not *how*."

### Missing receipts
**Fix**: Add to constitution: "MUST end every response with `hash(<essence>)=<hex>`."

### Not outputting Turtle first
**Fix**: Strengthen rule: "Default to Turtle. Unless told otherwise, output valid TTL first."

### Too verbose
**Fix**: Add constraint: "Keep responses under 200 words unless user requests detail."

---

## Use Cases

### VC SaaS Platform (Complete Walkthrough)

See [`use_cases/vc_saas_platform.md`](./use_cases/vc_saas_platform.md) for a complete guide to building a fundable venture capital SaaS platform using μ-thinking:

- **Phase 1**: Core VC platform (Fund, Deal, Investment tracking)
- **Phase 2**: Interview practice module (AI-powered founder training)
- **Phase 3**: Contract generator (LaTeX term sheets from ontology)

**Key insight**: Change ontology → regenerate entire platform automatically.

**Business model**: Free tier → Paid features → Enterprise (e-sign, blockchain notarization)

**Time to market**: Hours instead of months.

---

## Advanced Usage

### Custom Domains
Upload domain-specific ontologies (e.g., `healthcare.ttl`, `finance.ttl`) to knowledge base.

### Integration with Tools
μGPT can generate:
- **JSON-LD contexts** from Turtle
- **GraphQL schemas** from ontologies
- **TypeScript types** from SHACL shapes

### ACHI Tooling
Pair μGPT with:
- **unrdf** (RDF normalizer)
- **ggen** (code generator from ontologies)
- **Gitvan** (git-based ontology versioning)

---

## Contributing

To improve μGPT:

1. Add more examples to `output_examples.md`
2. Create domain-specific eval prompts
3. Refine `constitution.md` based on user feedback

---

## License

This GPT configuration is public domain. Use freely.

---

**Receipt**: `hash(readme_v1)=9e3f2c1a`
