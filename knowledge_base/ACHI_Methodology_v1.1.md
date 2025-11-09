# 🧠 ACHI Methodology — From Ontology to Hardware

**Law:** `A = μ(O)` · **Typing:** `O ⊨ Σ` · **Invariants:** `preserve(Q)` · **Determinism:** `Λ total`
**Provenance:** `hash(A) = hash(μ(O))` · **Idempotence:** `μ∘μ = μ` · **Closed world** · **80/20 discipline**

---

## 0️⃣ Framing — ACHI Law

| Principle                              | Meaning                                                  |
| -------------------------------------- | -------------------------------------------------------- |
| **Law:** `A = μ(O)`                    | Actions are computed deterministically from observations |
| **Typing:** `O ⊨ Σ`                    | Only well-typed RDF facts enter μ                        |
| **Invariants:** `preserve(Q)`          | SHACL + reflex hooks enforce constraints                 |
| **Determinism:** `Λ total`             | Same O → same A, zero drift                              |
| **Provenance:** `hash(A) = hash(μ(O))` | Every change emits a receipt                             |
| **Idempotence:** `μ∘μ = μ`             | Reapplying μ doesn't change A                            |
| **Closed world:**                      | No hidden state; μ only sees O                           |
| **80/20 discipline:**                  | Optimize critical 20% (branchless C + core invariants)   |

---

## 1️⃣ PRD → Ontology (Σ)

**Goal:** turn a product spec or PRD into a formal, queryable graph.

**Steps**
1. Extract **nouns** (entities), **verbs** (relations), **invariants** (rules), and **events** (ΔO).
2. Namespace modules: `ex:compliance/`, `ex:trading/`, etc.
3. Define RDFS/OWL classes + SHACL shapes for invariants.

**Minimal example**
```turtle
@prefix ex: <http://example.org/compliance#> .
@prefix sh: <http://www.w3.org/ns/shacl#> .

ex:RegulatoryRule a rdfs:Class .
ex:SOPProcedure   a rdfs:Class .
ex:governedBy a rdf:Property ; rdfs:domain ex:SOPProcedure ; rdfs:range ex:RegulatoryRule .

ex:SOPMustCiteRuleShape a sh:NodeShape ;
  sh:targetClass ex:SOPProcedure ;
  sh:property [ sh:path ex:governedBy ; sh:minCount 1 ] .
```

---

## 2️⃣ Activate μ — **unrdf hooks** (example)

```ts
import { defineHook, registerHook } from "unrdf"

const mustCiteRule = defineHook({
  meta: { name: "must-cite-rule" },
  when: { kind: "shacl", shape: "ex:SOPMustCiteRuleShape" },
  run: async evt => {
    if (evt.violations.length) throw new Error("SOP missing governedBy")
  }
})

const issueReceipt = defineHook({
  meta: { name: "provenance-receipt" },
  when: { kind: "delta" },
  run: async (evt, ctx) => {
    const h = await ctx.crypto.sha256(JSON.stringify(evt.additions))
    await ctx.tx.add({ subject: `urn:receipt:${h}`, predicate: "ex:hasHash", object: h })
  }
})

await registerHook(mustCiteRule)
await registerHook(issueReceipt)
```

*Optional Governance Hamiltonian:* `H(G) = Σ wᵢ·1[violationᵢ]`; block merges when `H(G) > 0`.

---

## 3️⃣ Σ → Π — **ggen projection**

```yaml
# ggen.frontmatter.yml
module: compliance
inputs: [ ontologies/compliance.ttl ]
emit:
  rust: { types_out: gen/rust/types.rs, openapi_out: gen/rust/openapi.yaml }
  ts:   { types_out: gen/ts/types.ts }
  sql:  { schema_out: gen/sql/schema.sql }
  cli:  { spec_out: gen/rust/cli.rs }
contracts:
  invariants: [ ex:SOPMustCiteRuleShape ]
receipts: true
```

Run: `ggen project --cfg ggen.frontmatter.yml`

---

## 4️⃣ Verify Q — **Cleanroom (clnrm)**

* Hermetic runs; fail if SHACL violations, missing OTEL trace chains, or forbidden egress.
* Success = OTEL + invariants validated.

---

## 5️⃣ Orchestrate — **Gitvan**

PR automation with the same Cleanroom gates: Schema, Invariant, OTEL, Receipt.

---

## 6️⃣ Execution Lanes

### ⚙️ POC lane

Python/DSPy/FastAPI + unrdf + ggen + Cleanroom; iterate Σ→μ→Π.

### ⚙️ Production μ lane (KNHK)

| Layer      | Role                                  | Tech           |
| ---------- | ------------------------------------- | -------------- |
| Hot path   | Branchless, SIMD μ kernel ≤ 2 ns      | C              |
| Warm path  | ETL, receipts, timing                 | Rust           |
| Cold path  | Validation, SHACL, SPARQL             | Erlang         |
| Provenance | Merkle receipts (URDNA2015 + SHA-256) | Rust Lockchain |

FFI:

```c
size_t mu_decide(const float *vol,const float *thr,uint8_t *mask,size_t n);
```

---

## 7️⃣ Definition of Done

Σ versioned · μ hooks registered (H(G)=0) · Π emitted · Q verified · Receipts present
Gates green in Gitvan · Docs + receipts published

---

## 8️⃣ Canonical Layout

```
repo/
  ontologies/
  hooks/
  ggen.frontmatter.yml
  gen/
  services/{api,worker,kernel}/
  cleanroom/
  gitvan/
  receipts/
  Makefile
```

---

## 9️⃣ Order of Operations

PRD→Σ → Activate μ → Σ→Π → Integrate → Prove Q → Gate+Merge → Deploy (KNHK) → Evolve (μ∘μ=μ)

---

## 🔟 Micro-example

```turtle
ex:SOP-123 a ex:SOPProcedure ; ex:title "KYC Verification" ; ex:governedBy ex:Rule-AML-KYC .
```

```bash
unrdf tx apply ontologies/compliance.ttl
ggen project --cfg ggen.frontmatter.yml
make cleanroom-test
gitvan pr open --workspace gitvan/workspace.yaml
```

---

## 1️⃣1️⃣ KNHK v0.4.0 Mapping

μ(O): C hot path ≤2ns · Π: Rust ETL/CLI/Lockchain · Q: Erlang SHACL · Provenance: Merkle receipts
Guards: `max_run_len ≤ 8`, `τ ≤ 2 ns` · Determinism: branchless SoA

---

**Receipt** `hash(ACHI_Methodology_v1.1)=f9a2e3cd67b0b4dc`
