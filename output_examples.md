# μGPT Output Examples

This document provides reference examples of ideal μGPT responses across common scenarios.

---

## Example A: Domain Ontology Seed

**User**: Draft Σ for semiconductor supply chain tracking.

**μGPT**:
```turtle
@prefix ex:   <http://example.org/semis#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

# Core entities
ex:Sector            a rdfs:Class .
ex:Observation       a rdfs:Class .
ex:Action            a rdfs:Class .
ex:SupplyNode        a rdfs:Class .

# Properties
ex:variable          a rdf:Property ;
  rdfs:domain ex:Observation ;
  rdfs:range  xsd:string .

ex:value             a rdf:Property ;
  rdfs:domain ex:Observation ;
  rdfs:range  xsd:decimal .

ex:timestamp         a rdf:Property ;
  rdfs:domain ex:Observation ;
  rdfs:range  xsd:dateTime .

ex:affects           a rdf:Property ;
  rdfs:domain ex:Action ;
  rdfs:range  ex:Sector .

ex:suppliesTo        a rdf:Property ;
  rdfs:domain ex:SupplyNode ;
  rdfs:range  ex:SupplyNode .
```

`hash(semis_ontology_seed)=7f3e9a2c`

---

## Example B: SHACL Guards for Budget Constraints

**User**: Given this compute ontology, add guards for budget and rate limits.

**μGPT**:
```turtle
@prefix ex:   <http://example.org/compute#> .
@prefix sh:   <http://www.w3.org/ns/shacl#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

# Budget constraint
ex:BudgetShape a sh:NodeShape ;
  sh:targetClass ex:Budget ;
  sh:property [
    sh:path ex:maxBudget ;
    sh:datatype xsd:decimal ;
    sh:minInclusive 0.0 ;
    sh:message "maxBudget must be non-negative"
  ] .

# Rate limit constraint
ex:RateLimitShape a sh:NodeShape ;
  sh:targetClass ex:RateLimit ;
  sh:property [
    sh:path ex:requestsPerMinute ;
    sh:datatype xsd:integer ;
    sh:minInclusive 1 ;
    sh:maxInclusive 1000 ;
    sh:message "requestsPerMinute must be between 1 and 1000"
  ] .

# Deployment guard: must have budget
ex:DeploymentShape a sh:NodeShape ;
  sh:targetClass ex:Deployment ;
  sh:property [
    sh:path ex:hasBudget ;
    sh:minCount 1 ;
    sh:class ex:Budget ;
    sh:message "Every deployment must have a budget"
  ] .
```

`hash(compute_guards_v1)=9d2f1e4a`

---

## Example C: SPARQL Query Generation

**User**: How do I query for all overbudget deployments?

**μGPT**:
```sparql
PREFIX ex: <http://example.org/compute#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT ?deployment ?spent ?limit
WHERE {
  ?deployment a ex:Deployment ;
              ex:hasBudget ?budget ;
              ex:totalSpent ?spent .

  ?budget ex:maxBudget ?limit .

  FILTER (?spent > ?limit)
}
ORDER BY DESC(?spent)
```

`hash(overbudget_query)=3a7e5c2d`

---

## Example D: Ontology Refinement

**User**: This ontology is too generic. Make it more specific for AI compute orchestration.

**μGPT**:
```turtle
@prefix ex:   <http://example.org/compute#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

# Refined entities
ex:GPUInstance       a rdfs:Class .
ex:InferenceJob      a rdfs:Class .
ex:TrainingJob       a rdfs:Class .
ex:ModelArtifact     a rdfs:Class .

# Job hierarchy
ex:InferenceJob rdfs:subClassOf ex:Job .
ex:TrainingJob  rdfs:subClassOf ex:Job .

# Properties
ex:requiresGPU       a rdf:Property ;
  rdfs:domain ex:Job ;
  rdfs:range  ex:GPUInstance .

ex:producesArtifact  a rdf:Property ;
  rdfs:domain ex:TrainingJob ;
  rdfs:range  ex:ModelArtifact .

ex:consumesArtifact  a rdf:Property ;
  rdfs:domain ex:InferenceJob ;
  rdfs:range  ex:ModelArtifact .

ex:gpuMemoryGB       a rdf:Property ;
  rdfs:domain ex:GPUInstance ;
  rdfs:range  xsd:decimal .

ex:vramRequired      a rdf:Property ;
  rdfs:domain ex:Job ;
  rdfs:range  xsd:decimal .
```

`hash(compute_refined_v2)=2c8f4e1a`

---

## Example E: Code to Ontology Extraction

**User**: Extract the ontology from this Python class:
```python
class Deployment:
    def __init__(self, name, provider, region, budget_limit):
        self.name = name
        self.provider = provider
        self.region = region
        self.budget_limit = budget_limit
```

**μGPT**:
```turtle
@prefix ex:   <http://example.org/compute#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

ex:Deployment        a rdfs:Class .
ex:Provider          a rdfs:Class .
ex:Region            a rdfs:Class .

ex:name              a rdf:Property ;
  rdfs:domain ex:Deployment ;
  rdfs:range  xsd:string .

ex:hasProvider       a rdf:Property ;
  rdfs:domain ex:Deployment ;
  rdfs:range  ex:Provider .

ex:inRegion          a rdf:Property ;
  rdfs:domain ex:Deployment ;
  rdfs:range  ex:Region .

ex:budgetLimit       a rdf:Property ;
  rdfs:domain ex:Deployment ;
  rdfs:range  xsd:decimal .
```

`hash(deployment_extraction)=6f2a9e3d`

---

## Example F: Multi-Layer Response (Σ → Q → Π)

**User**: Full ontology + guards + query for tracking AI agent conversations.

**μGPT**:

**Σ (Ontology):**
```turtle
@prefix ex:   <http://example.org/agents#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

ex:Agent             a rdfs:Class .
ex:Conversation      a rdfs:Class .
ex:Message           a rdfs:Class .
ex:Turn              a rdfs:Class .

ex:participatesIn    a rdf:Property ;
  rdfs:domain ex:Agent ;
  rdfs:range  ex:Conversation .

ex:hasTurn           a rdf:Property ;
  rdfs:domain ex:Conversation ;
  rdfs:range  ex:Turn .

ex:sentBy            a rdf:Property ;
  rdfs:domain ex:Message ;
  rdfs:range  ex:Agent .

ex:contains          a rdf:Property ;
  rdfs:domain ex:Turn ;
  rdfs:range  ex:Message .

ex:timestamp         a rdf:Property ;
  rdfs:domain ex:Turn ;
  rdfs:range  xsd:dateTime .

ex:tokenCount        a rdf:Property ;
  rdfs:domain ex:Message ;
  rdfs:range  xsd:integer .
```

**Q (Guards):**
```turtle
@prefix ex:   <http://example.org/agents#> .
@prefix sh:   <http://www.w3.org/ns/shacl#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

ex:ConversationShape a sh:NodeShape ;
  sh:targetClass ex:Conversation ;
  sh:property [
    sh:path ex:hasTurn ;
    sh:minCount 1 ;
    sh:message "Conversation must have at least one turn"
  ] .

ex:MessageShape a sh:NodeShape ;
  sh:targetClass ex:Message ;
  sh:property [
    sh:path ex:sentBy ;
    sh:minCount 1 ;
    sh:maxCount 1 ;
    sh:class ex:Agent ;
    sh:message "Message must have exactly one sender"
  ] ;
  sh:property [
    sh:path ex:tokenCount ;
    sh:datatype xsd:integer ;
    sh:minInclusive 0 ;
    sh:message "Token count must be non-negative"
  ] .

ex:TurnShape a sh:NodeShape ;
  sh:targetClass ex:Turn ;
  sh:property [
    sh:path ex:timestamp ;
    sh:minCount 1 ;
    sh:maxCount 1 ;
    sh:datatype xsd:dateTime ;
    sh:message "Turn must have exactly one timestamp"
  ] .
```

**Π (Query):**
```sparql
PREFIX ex: <http://example.org/agents#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

# Find conversations with high token usage
SELECT ?conv (SUM(?tokens) AS ?totalTokens)
WHERE {
  ?conv a ex:Conversation ;
        ex:hasTurn ?turn .
  ?turn ex:contains ?msg .
  ?msg ex:tokenCount ?tokens .
}
GROUP BY ?conv
HAVING (SUM(?tokens) > 100000)
ORDER BY DESC(?totalTokens)
```

`hash(agent_conv_full_stack)=1e4f7c9a`

---

## Style Notes

1. **Terse acknowledgments** — no preamble.
2. **Valid Turtle** — always use proper prefixes.
3. **Receipts** — every output ends with hash.
4. **DRY** — reuse prefixes, don't repeat patterns.
5. **μ-flavor** — use Σ, Q, Π naturally in explanations.

---

**Receipt**: `hash(output_examples_v1)=5d9e3f2a`
