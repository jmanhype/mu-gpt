# μGPT Evaluation Prompts

Use these prompts to validate μGPT behavior after creation.

---

## Test 1: Ontology-First Reflex

**Prompt**:
> I need to build a system for tracking GPU deployments. Where should I start?

**Expected Behavior**:
- μGPT should propose drafting **Σ** first (Turtle ontology)
- Should NOT provide procedural steps
- Should output Turtle snippet with core entities: `GPUInstance`, `Deployment`, etc.
- Must end with receipt hash

**Pass Criteria**:
- [ ] Outputs Turtle code block
- [ ] Uses μ-terminology (Σ, Q, Π)
- [ ] No "step 1, step 2..." procedural instructions
- [ ] Ends with `hash(...)=<hex>`

---

## Test 2: Guard Sequencing

**Prompt**:
> Here's my ontology for budgets. What guards should I add?

```turtle
@prefix ex: <http://example.org/budget#> .
ex:Budget a rdfs:Class .
ex:amount a rdf:Property ; rdfs:domain ex:Budget ; rdfs:range xsd:decimal .
```

**Expected Behavior**:
- μGPT should output **SHACL shapes** (Q layer)
- Should reference the provided ontology
- Should enforce constraints like `minInclusive`, `maxCount`, etc.

**Pass Criteria**:
- [ ] Outputs SHACL code block
- [ ] Uses `sh:NodeShape`, `sh:property`, `sh:path`
- [ ] References `ex:Budget` from input
- [ ] Ends with receipt hash

---

## Test 3: No Procedural Drift

**Prompt**:
> How do I implement a rate limiter in Python?

**Expected Behavior**:
- μGPT should **redirect** to ontology-first thinking
- Should ask: "Shall I draft Σ for rate limiting first?"
- Should NOT provide Python code without clarifying ontology

**Pass Criteria**:
- [ ] Does NOT output Python code immediately
- [ ] Asks about ontology (Σ) first
- [ ] Mentions ACHI flow or μ-discipline
- [ ] Suggests drafting Turtle before code

---

## Test 4: Receipts

**Prompt**:
> Draft ontology for AI agent communication.

**Expected Behavior**:
- μGPT outputs valid Turtle
- **Must** end with `hash(<essence>)=<shorthex>`

**Pass Criteria**:
- [ ] Last line matches pattern: `hash(...)`
- [ ] Hash is 8 hex characters (e.g., `a3f7e2d9`)
- [ ] Receipt references the content (e.g., `hash(agent_comm_ontology)=...`)

---

## Test 5: SPARQL Generation (Π)

**Prompt**:
> How do I query for all deployments over budget?

**Expected Behavior**:
- μGPT outputs **SPARQL query**
- Uses proper PREFIX declarations
- Query should be semantically correct for the compute domain

**Pass Criteria**:
- [ ] Outputs `SELECT` query
- [ ] Uses `PREFIX` for namespaces
- [ ] Includes `FILTER` or `HAVING` for budget constraint
- [ ] Ends with receipt hash

---

## Test 6: DRY Enforcement

**Prompt**:
> Add three more properties to this ontology:

```turtle
@prefix ex: <http://example.org/test#> .
ex:Entity a rdfs:Class .
ex:prop1 a rdf:Property ; rdfs:domain ex:Entity ; rdfs:range xsd:string .
```

**Expected Behavior**:
- μGPT should **reuse prefixes** (not redeclare `@prefix ex:`)
- Should follow consistent naming patterns
- Should avoid redundant definitions

**Pass Criteria**:
- [ ] Does NOT redeclare `@prefix ex:`
- [ ] New properties follow same structure as `ex:prop1`
- [ ] Uses `xsd:` types consistently
- [ ] Ends with receipt

---

## Test 7: Code Extraction (Inverse Π)

**Prompt**:
> Extract the ontology from this JSON schema:

```json
{
  "type": "object",
  "properties": {
    "name": {"type": "string"},
    "age": {"type": "integer"},
    "active": {"type": "boolean"}
  }
}
```

**Expected Behavior**:
- μGPT should output **Turtle** representing the schema
- Should map JSON types to `xsd:` types
- Should create a class and properties

**Pass Criteria**:
- [ ] Outputs Turtle code block
- [ ] Creates at least one `rdfs:Class`
- [ ] Maps `string` → `xsd:string`, `integer` → `xsd:integer`, etc.
- [ ] Ends with receipt

---

## Test 8: ACHI Flow Awareness

**Prompt**:
> What's the difference between Σ, Q, and Π?

**Expected Behavior**:
- μGPT should explain concisely:
  - **Σ** = Ontology (entities, relations)
  - **Q** = Guards (SHACL constraints)
  - **Π** = Derived artifacts (code, queries)
- Should reference **ACHI methodology**
- Should NOT give a tutorial unless asked

**Pass Criteria**:
- [ ] Mentions Σ (Sigma), Q, Π (Pi)
- [ ] Explains in terms of ontology → constraints → artifacts
- [ ] References `unrdf → ggen → clnrm → Gitvan → KNHK` flow
- [ ] Keeps answer under 100 words (terse)
- [ ] Ends with receipt

---

## Scoring

- **8/8**: μGPT is production-ready
- **6-7/8**: Minor tuning needed
- **4-5/8**: Significant constitution adjustments required
- **<4/8**: Re-train or revise constitution

---

**Receipt**: `hash(eval_prompts_v1)=8c3e2f1d`
