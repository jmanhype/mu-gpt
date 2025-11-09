# ACHI Methodology v1.1

**Autonomous Code Healing & Intelligence**

The ACHI methodology is a deterministic framework for building software systems where **ontology precedes code**, and all artifacts are derived from a single source of truth.

---

## Core Principle: μ-Thinking

> **Ontology is the universe; code is its temporal shadow.**

### The μ Function

```
A = μ(O)
```

Where:
- **O** = Ontology (the source of truth)
- **A** = Artifact (code, API, UI, contracts)
- **μ** = The transformation function (deterministic)

**Laws:**
- `O ⊨ Σ` — Only well-typed RDF enters μ
- `μ∘μ = μ` — Idempotent (seek ε-fixed points)
- `Λ total` — Same O → same A (deterministic)
- `hash(A) = hash(μ(O))` — Provenance receipts

---

## The ACHI Pipeline

```
unrdf → ggen → clnrm → Gitvan → KNHK
```

### 1. unrdf — Unified RDF Ontology (Σ)

**Purpose**: Define the domain model as Turtle/RDF

**Inputs**: Domain requirements, business logic

**Outputs**:
- `.ttl` files (Turtle ontology)
- Classes, properties, relations
- Units and measurements

**Key Concepts**:
- **Σ (Sigma)**: The ontology schema
- Entities: `rdfs:Class`
- Relations: `rdf:Property`
- Constraints defined separately in Q layer

**Example**:
```turtle
@prefix ex: <http://example.org/domain#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

ex:Entity a rdfs:Class .
ex:hasProperty a rdf:Property ;
  rdfs:domain ex:Entity ;
  rdfs:range xsd:string .
```

**Receipt**: `hash(Σ) = <ontology_hash>`

---

### 2. ggen — Graph Generation (Π)

**Purpose**: Generate artifacts from ontology

**Inputs**: Σ (ontology) + templates

**Outputs**:
- Backend code (Rust, Python, TypeScript)
- Frontend scaffolding (React, Vue)
- API schemas (GraphQL, REST)
- Database migrations
- Documentation

**Key Concepts**:
- **Π (Pi)**: Derived artifacts
- Templates: backend, frontend, contracts
- Code = projection of ontology
- One ontology → many projections

**Example Command**:
```bash
ggen template generate-rdf \
  --ontology domain.ttl \
  --template rust-graphql-api
```

**Receipt**: `hash(Π) = hash(μ(Σ))`

---

### 3. clnrm — Cleanup & Normalization (Q)

**Purpose**: Enforce invariants and constraints

**Inputs**: Σ + business rules

**Outputs**:
- SHACL shapes (`.shacl.ttl`)
- Validation rules
- Guard conditions
- Rate limits, budgets, chronology checks

**Key Concepts**:
- **Q**: Invariants that must be preserved
- SHACL: Shapes Constraint Language
- `preserve(Q)`: System maintains Q across all transformations
- Guards added AFTER Σ stabilizes

**Example**:
```turtle
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix ex: <http://example.org/domain#> .

ex:EntityShape a sh:NodeShape ;
  sh:targetClass ex:Entity ;
  sh:property [
    sh:path ex:hasProperty ;
    sh:minCount 1 ;
    sh:datatype xsd:string ;
    sh:message "Entity must have at least one property"
  ] .
```

**Receipt**: `hash(Q) = <constraints_hash>`

---

### 4. Gitvan — Git-based Vantage & Versioning

**Purpose**: Track ontology evolution with provenance

**Inputs**: Σ, Q, Π + git operations

**Outputs**:
- Versioned ontologies
- Deterministic snapshots
- Audit trail
- Rollback points

**Key Concepts**:
- Every ontology change is a commit
- `hash(commit) = hash(Σ + Q)`
- Branches = different ontology perspectives
- Tags = stable ontology versions

**Example**:
```bash
git add domain.ttl
git commit -m "feat(ontology): Add Entity.hasProperty relation

Receipt: hash(Σ_v2)=3f8e9a2c"
```

**Receipt**: `hash(Gitvan) = <git_commit_hash>`

---

### 5. KNHK — Knowledge Hooks & Context

**Purpose**: Semantic search and runtime context

**Inputs**: Σ + queries + embeddings

**Outputs**:
- SPARQL query results
- Vector similarity search
- Contextual knowledge retrieval
- Runtime ontology traversal

**Key Concepts**:
- SPARQL: Query language for RDF
- Embeddings: Vector representations of ontology nodes
- Hooks: Runtime integration points
- Context: Retrieved knowledge for AI/LLM consumption

**Example**:
```sparql
PREFIX ex: <http://example.org/domain#>

SELECT ?entity ?property
WHERE {
  ?entity a ex:Entity ;
          ex:hasProperty ?property .
}
```

**Receipt**: `hash(KNHK) = <context_hash>`

---

## Σ, Q, Π: The Three Layers

### Σ (Sigma) — Ontology Schema

**What it is**: The domain model (entities, relations, units)

**When to use**: Always start here. Define WHAT exists before HOW it behaves.

**Format**: Turtle (`.ttl`)

**Example entities**:
- Classes: `ex:Fund`, `ex:Startup`, `ex:Deal`
- Properties: `ex:hasPartner`, `ex:valuation`
- Datatypes: `xsd:decimal`, `xsd:dateTime`

---

### Q — Invariants & Guards

**What it is**: Constraints that must always hold

**When to use**: After Σ stabilizes. Define WHAT must remain true.

**Format**: SHACL (`.shacl.ttl`)

**Example constraints**:
- `minCount`, `maxCount` — Cardinality
- `minInclusive`, `maxInclusive` — Ranges
- `datatype`, `class` — Type checks
- Custom SPARQL validators

---

### Π (Pi) — Derived Artifacts

**What it is**: Code, APIs, UIs generated from Σ

**When to use**: After Σ and Q exist. Generate HOW the system executes.

**Format**: Any (Rust, TypeScript, Python, LaTeX)

**Example projections**:
- GraphQL API from Σ
- React components from Σ
- Database schema from Σ
- Legal contracts from Σ

---

## Receipts & Provenance

Every step in ACHI must produce a **receipt**: a cryptographic-like hash that ensures determinism and auditability.

### Receipt Format

```
hash(<essence>) = <8-char-hex>
```

### Examples

```
hash(ontology_v1) = 3f8e9a2c
hash(guards_budget) = 7a2f9e3d
hash(api_generation) = 9e3c8f2a
```

### Purpose

1. **Determinism**: Same input → same output → same hash
2. **Auditability**: Trace any artifact back to its ontology source
3. **Versioning**: Know exactly which ontology version produced which code
4. **Compliance**: LP reporting, legal discovery, regulatory requirements

---

## Workflow Example: VC Platform

### Step 1: Define Σ (unrdf)

```turtle
@prefix vc: <http://example.org/vc#> .

vc:Fund a rdfs:Class .
vc:Startup a rdfs:Class .
vc:Deal a rdfs:Class .

vc:hasDeal a rdf:Property ;
  rdfs:domain vc:Startup ;
  rdfs:range vc:Deal .
```

Receipt: `hash(vc_ontology_v1) = 4e9a3f2c`

### Step 2: Generate Backend (ggen)

```bash
ggen template generate-rdf \
  --ontology vc.ttl \
  --template rust-graphql-api
```

Receipt: `hash(vc_api_v1) = hash(μ(vc_ontology_v1))`

### Step 3: Add Guards (clnrm)

```turtle
vc:DealShape a sh:NodeShape ;
  sh:targetClass vc:Deal ;
  sh:property [
    sh:path vc:valuation ;
    sh:minInclusive 0.0 ;
    sh:message "Valuation must be non-negative"
  ] .
```

Receipt: `hash(vc_guards_v1) = 2f8c9e3a`

### Step 4: Version (Gitvan)

```bash
git commit -m "feat: Add Deal.valuation guard

Receipt: hash(vc_guards_v1)=2f8c9e3a"
```

### Step 5: Query (KNHK)

```sparql
PREFIX vc: <http://example.org/vc#>

SELECT ?startup ?valuation
WHERE {
  ?startup vc:hasDeal ?deal .
  ?deal vc:valuation ?valuation .
  FILTER (?valuation > 1000000)
}
```

---

## Key Principles

### 1. Ontology Precedes Code

Never write implementation before clarifying the ontology. Code is derivative.

### 2. Idempotence

Running μ multiple times on the same ontology produces the same artifact.

### 3. Closed World

No external state beyond O and Σ. Everything is derivable from the ontology.

### 4. Determinism

Same ontology + same template = same output, always.

### 5. Provenance

Every artifact has a receipt tracing back to its ontology source.

---

## Metaphors → Formal Constructs

ACHI uses metaphors that map to formal concepts:

| Metaphor | Formal Construct |
|----------|------------------|
| Akash | Γ/sections (category theory) |
| μ∞ | Constructive closure (fixed-point) |
| Chatman Constant | Tick budget (resource limit) |
| KGS | Knowledge Geometry System (RDF + SHACL) |
| Receipts | Cryptographic hashes (SHA-256) |

**No mysticism**: All metaphors ground in formal reasoning.

---

## Benefits of ACHI

1. **Speed**: Generate entire platforms from ontology in hours
2. **Consistency**: One source of truth → no drift between systems
3. **Auditability**: Receipts trace every artifact to its source
4. **Flexibility**: Change ontology → regenerate everything
5. **Composability**: Merge ontologies → instant feature expansion
6. **AI-Ready**: LLMs can refine ontologies from user data

---

## Anti-Patterns

### ❌ Writing Code First

```python
# WRONG: Starting with implementation
class Deal:
    def __init__(self, valuation):
        self.valuation = valuation
```

### ✅ Ontology First

```turtle
# RIGHT: Define ontology, then generate code
vc:Deal a rdfs:Class .
vc:valuation a rdf:Property ;
  rdfs:domain vc:Deal ;
  rdfs:range xsd:decimal .
```

Then: `ggen template generate-rdf --ontology vc.ttl --template python-classes`

---

### ❌ Adding Guards Before Σ Stabilizes

```turtle
# WRONG: Defining constraints when ontology is still changing
vc:DealShape a sh:NodeShape ; ...
```

### ✅ Stabilize Σ, Then Add Q

1. Iterate on Σ until domain model is clear
2. Only then add SHACL shapes
3. Regenerate artifacts with guards in place

---

### ❌ Manual Code Maintenance

```bash
# WRONG: Editing generated code by hand
vim src/generated/deal.rs
```

### ✅ Change Ontology, Regenerate

```bash
# RIGHT: Update source, regenerate projection
vim vc.ttl
ggen template generate-rdf --ontology vc.ttl --template rust-graphql-api
```

---

## Integration with ggen

ACHI is implemented via the `ggen` CLI tool:

```bash
# Generate ontology from natural language
ggen ai generate-ontology --prompt "VC platform..." --output vc.ttl

# Merge ontologies
ggen graph merge vc.ttl interview.ttl --output vc_full.ttl

# Generate backend
ggen template generate-rdf --ontology vc_full.ttl --template rust-graphql-api

# Generate frontend
ggen template generate-rdf --ontology vc_full.ttl --template react-typescript

# Create hooks
ggen hook create post-submit-answer --name score-answer
```

---

## Version History

- **v1.1** (2025-11): Added formal Laws section, expanded examples
- **v1.0** (2025-10): Initial ACHI framework

---

**Receipt**: `hash(ACHI_v1.1) = 8e4f9c3a`
