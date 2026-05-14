# Job & Career Recommendation Ontology

> **Course:** Knowledge Engineering and Ontologies
> **University:** Manisa Celal Bayar Üniversitesi
> **Term:** Spring 2026
> **Authors:** Naciye Nur Akkuş (220315039) · Elif Sude Yılmaz (220315022)
> **Documentation:** https://naciyenurrr.github.io/job-ontology/

---

## What is this?

This project is an OWL 2 ontology we built for the Knowledge Engineering and Ontologies course. The domain we chose is **job recruitment** — the idea is to formally represent candidates, job postings, companies, skills, and experience levels in a way that a reasoner can use to automatically match candidates to jobs, find skill gaps, and recommend positions.

We started with a basic version in Phase 1, then extended it significantly in Phase 2 by adding skill proficiency levels, certifications, sector classification, and alignment with external vocabularies like ESCO and schema.org.

---

## Version History

| Version | Date | What changed |
|---|---|---|
| v1.0 | April 2026 | Initial ontology: 10 classes, 10 object properties, 7 datatype properties, 3 candidates, 3 jobs |
| v2.0 | May 2026 | +4 classes (SkillProficiency, SkillAssessment, Certification, Sector), +9 object properties, +6 datatype properties, 2 new candidates, 2 new jobs, 3 certifications, schema.org + ESCO alignment |

---

## Repository Structure

```
job-ontology/
│
├── job-ontology-v2.ttl       # Current ontology file (OWL 2 DL, Turtle) — use this one
├── job-ontology-v1.ttl       # Archived v1.0 for comparison
├── index.html                # Ontology documentation (published via GitHub Pages)
├── ORSD_v2.docx              # Ontology Requirements Specification Document, Version 2
├── Phase2_Report.docx        # Full Phase 2 project report
├── README.md                 # This file
└── sparql/
    ├── cq1_candidate_skills.sparql
    ├── cq4_job_matching.sparql
    ├── cq11_skill_gap.sparql
    ├── cq13_proficiency.sparql
    └── cq16_certification_match.sparql
```

---

## Classes

### Core classes (v1.0)

| Class | Subclass of | Description |
|---|---|---|
| `Person` | — | Anyone in the recruitment process |
| `Candidate` | `Person` ⊥ `Recruiter` | A job seeker. Disjoint with Recruiter |
| `Recruiter` | `Person` | Manages job postings and evaluates candidates |
| `Job` | — | A specific position advertised by a company |
| `Company` | — | An organisation that posts jobs |
| `Skill` | — | A competency relevant to employment |
| `TechnicalSkill` | `Skill` | Programming, tools, engineering skills |
| `SoftSkill` | `Skill` | Interpersonal and communication abilities |
| `ExperienceLevel` | — | Seniority: Junior / Mid / Senior |
| `JobCategory` | — | Broad occupational grouping |

### New in v2.0

| Class | Description |
|---|---|
| `SkillProficiency` | Mastery level for a skill: Beginner / Intermediate / Expert |
| `SkillAssessment` | Reification node linking a candidate, a skill, and a proficiency level together |
| `Certification` | A formal credential from a recognised issuing body (Google, AWS, etc.) |
| `Sector` | Industry vertical: Technology, FinTech, HealthTech, E-Commerce |

---

## Object Properties

| Property | Domain | Range | Note |
|---|---|---|---|
| `hasSkill` | Candidate | Skill | inverse: `isSkillOf` |
| `isSkillOf` | Skill | Candidate | inverse: `hasSkill` |
| `requiresSkill` | Job | Skill | |
| `offeredBy` | Job | Company | inverse: `offersJob` |
| `offersJob` | Company | Job | inverse: `offeredBy` |
| `appliesTo` | Candidate | Job | |
| `recommendedFor` | Job | Candidate | |
| `hasExperienceLevel` | Candidate | ExperienceLevel | |
| `requiresExperienceLevel` | Job | ExperienceLevel | |
| `belongsToCategory` | Job | JobCategory | |
| `hasSkillAssessment` *(v2)* | Candidate | SkillAssessment | |
| `assessedSkill` *(v2)* | SkillAssessment | Skill | |
| `hasProficiency` *(v2)* | SkillAssessment | SkillProficiency | |
| `holdsCertification` *(v2)* | Candidate | Certification | |
| `certifiesSkill` *(v2)* | Certification | Skill | |
| `requiresCertification` *(v2)* | Job | Certification | |
| `operatesInSector` *(v2)* | Company | Sector | |
| `targetsSector` *(v2)* | Job | Sector | |

---

## Datatype Properties

| Property | Domain | Range |
|---|---|---|
| `name` | Person | xsd:string |
| `email` | Person | xsd:string |
| `jobTitle` | Job | xsd:string |
| `jobDescription` | Job | xsd:string |
| `companyName` | Company | xsd:string |
| `location` | Company | xsd:string |
| `yearsOfExperience` | Candidate | xsd:int |
| `certificationName` *(v2)* | Certification | xsd:string |
| `issuingBody` *(v2)* | Certification | xsd:string |
| `issueDate` *(v2)* | Certification | xsd:date |
| `expiryDate` *(v2)* | Certification | xsd:date |
| `salary` *(v2)* | Job | xsd:string |
| `remote` *(v2)* | Job | xsd:boolean |

---

## Competency Questions

| ID | Question |
|---|---|
| CQ1 | What skills does a given candidate possess? |
| CQ2 | What skills are required for a given job position? |
| CQ3 | Which candidates have all the skills required by a given job? |
| CQ4 | Which jobs match the skills of a given candidate? |
| CQ5 | What is the experience level of a given candidate? |
| CQ6 | What experience level does a given job require? |
| CQ7 | Which company offers a given job? |
| CQ8 | Which jobs does a given company offer? |
| CQ9 | Has a given candidate applied for a given job? |
| CQ10 | Which candidates are recommended for a given job? |
| CQ11 | What skills is a candidate missing for a given job (skill gap)? |
| CQ12 | How many years of experience does a candidate have? |
| CQ13 *(v2)* | What is the proficiency level of a candidate for a given skill? |
| CQ14 *(v2)* | Which certifications does a given candidate hold? |
| CQ15 *(v2)* | Which jobs require a specific certification? |
| CQ16 *(v2)* | Which candidates hold all certifications required by a given job? |

---

## Example SPARQL Queries

### CQ4 — Jobs matching Naciye's skills

```sparql
PREFIX : <http://www.example.org/jobontology#>

SELECT ?job ?title WHERE {
  :Naciye :hasSkill ?skill .
  ?job :requiresSkill ?skill ;
       :jobTitle ?title .
}
GROUP BY ?job ?title
HAVING (COUNT(?skill) = (
  SELECT (COUNT(?rs) AS ?reqCount) WHERE {
    ?job :requiresSkill ?rs .
  }
))
```

### CQ11 — Skill gap: Naciye vs DataScientistJob

```sparql
PREFIX : <http://www.example.org/jobontology#>

SELECT ?missingSkill WHERE {
  :DataScientistJob :requiresSkill ?missingSkill .
  FILTER NOT EXISTS { :Naciye :hasSkill ?missingSkill }
}
```

### CQ13 — Zeynep's proficiency per skill (v2)

```sparql
PREFIX : <http://www.example.org/jobontology#>

SELECT ?skill ?level WHERE {
  :Zeynep :hasSkillAssessment ?sa .
  ?sa :assessedSkill ?skill ;
      :hasProficiency ?level .
}
```

### CQ16 — Candidates holding all certs required by MLEngineerJob (v2)

```sparql
PREFIX : <http://www.example.org/jobontology#>

SELECT ?candidate WHERE {
  {
    SELECT (COUNT(?cert) AS ?total) WHERE {
      :MLEngineerJob :requiresCertification ?cert .
    }
  }
  ?candidate a :Candidate ;
             :holdsCertification ?heldCert .
  :MLEngineerJob :requiresCertification ?heldCert .
}
GROUP BY ?candidate
HAVING (COUNT(?heldCert) = ?total)
```

---

## Instance Data

### Candidates

| Name | Skills | Level | Exp | Certifications |
|---|---|---|---|---|
| Naciye Nur Akkuş | Python, DataAnalysis, DataVisualization, Tableau, SQL | Junior | 1 yr | Tableau Desktop Specialist |
| Berat Yılmaz | Python, MachineLearning, SQL, DataAnalysis | Mid | 3 yrs | — |
| Zeynep Arslan | Python, ML, DeepLearning, SQL, CloudComputing | Senior | 7 yrs | Google ML Engineer, AWS DA Specialty |
| Cem Kaya *(v2)* | Python, SQL, Spark, CloudComputing | Mid | 4 yrs | AWS DA Specialty |
| Selin Demir *(v2)* | Python, NLP, DeepLearning, MachineLearning | Senior | 6 yrs | — |

### Jobs

| Job | Company | Required Skills | Level | Remote |
|---|---|---|---|---|
| Data Scientist | TechCorp | Python, ML, SQL | Mid | No |
| Junior Data Analyst | DataHub | DataAnalysis, SQL, Tableau | Junior | Yes |
| ML Engineer | TechCorp | Python, ML, DeepLearning, Cloud | Senior | No |
| Data Engineer *(v2)* | FinAI | Python, SQL, Spark, Cloud | Mid | Yes |
| NLP Research Scientist *(v2)* | FinAI | Python, NLP, DeepLearning, ML | Senior | Yes |

---

## How to Load and Use

### Protégé
1. Open Protégé 5.x
2. `File → Open` → select `job-ontology-v2.ttl`
3. Start the **HermiT** reasoner (`Reasoner → Start Reasoner`) to check consistency and infer new facts

### Apache Jena (SPARQL)
```bash
sparql --data job-ontology-v2.ttl --query sparql/cq4_job_matching.sparql
```

### Python (rdflib)
```python
from rdflib import Graph

g = Graph()
g.parse("job-ontology-v2.ttl", format="turtle")

# CQ11 — skill gap for Naciye vs DataScientistJob
q = """
PREFIX : <http://www.example.org/jobontology#>
SELECT ?missingSkill WHERE {
  :DataScientistJob :requiresSkill ?missingSkill .
  FILTER NOT EXISTS { :Naciye :hasSkill ?missingSkill }
}
"""
for row in g.query(q):
    print(row.missingSkill)
```

---

## Ontology Development Methodology

We followed **METHONTOLOGY** across both phases:

| Phase | What we did |
|---|---|
| Specification | Wrote ORSD v1 and v2; defined competency questions |
| Conceptualisation | Sketched class hierarchies and property diagrams on paper first |
| Formalisation | Translated to OWL 2 DL axioms: subclasses, disjointness, domain/range |
| Implementation | Serialised in Turtle; manually validated syntax |
| Evaluation | Ran HermiT reasoner; tested all CQs with SPARQL |
| Maintenance | Version controlled on GitHub; ORSD updated for v2 |

---

## External Ontologies Used

| Ontology | Source | How we used it |
|---|---|---|
| ESCO | https://esco.ec.europa.eu | Aligned our `:Skill` individuals via `skos:exactMatch` |
| schema.org | https://schema.org | Annotated `:Job`, `:Person`, `:Company` with schema.org equivalents |
| FOAF | http://xmlns.com/foaf/0.1/ | Used `foaf:name`, `foaf:mbox` on candidate/recruiter profiles |
| Dublin Core | http://purl.org/dc/elements/1.1/ | Ontology metadata (title, creator, date, version) |
| SKOS | https://www.w3.org/2004/02/skos/core | `skos:definition` on all named individuals; `skos:exactMatch` for ESCO |

---

## Design Decisions

| Decision | Why |
|---|---|
| `Candidate` disjoint with `Recruiter` | Prevents a reasoner from accepting someone typed as both |
| Inverse properties (`hasSkill`/`isSkillOf`, `offeredBy`/`offersJob`) | Lets us traverse the graph in both directions without duplicating triples |
| `TechnicalSkill` and `SoftSkill` as subclasses | Finer queries without making the hierarchy too deep |
| `SkillAssessment` as a reification node (v2) | A plain `hasSkill` triple can't carry a proficiency level — the reified node solves this cleanly |
| `recommendedFor` as explicit assertion (v1) | In future versions this will be derived by a SPARQL CONSTRUCT rule or SWRL axiom |
| OWL 2 DL instead of Full | Keeps the ontology decidable for automated reasoning |

---

## Contributors

- **Naciye Nur Akkuş** — Ontology design, instance data, ORSD, documentation
- **Elif Sude Yılmaz** — Ontology design, SPARQL queries, GitHub setup, report writing
