# Job & Career Recommendation Ontology

> **Course:** Knowledge Engineering and Ontologies  
> **University:** [Your University Name]  
> **Term:** Spring 2026  
> **Authors:** Naciye Nur Akkuş · Elif Sude Yılmaz  

---

## Overview

This repository contains an OWL 2 ontology that formally models the **job recruitment domain**. The ontology enables intelligent, skill-based matching between job candidates and job postings, providing a semantic foundation for automated recommendation and skill gap analysis systems.

The ontology is serialised in **Turtle (.ttl)** format and is compliant with the **OWL 2 DL** profile, ensuring decidability and compatibility with standard OWL reasoners such as HermiT and Pellet.

---

## Objectives

- Formally represent the key entities of the recruitment domain: candidates, jobs, companies, skills, and experience levels.
- Enable **skill-based matching** between candidates and job postings using semantic reasoning.
- Support **skill gap analysis** to guide candidates' professional development.
- Provide a reusable, extensible knowledge base for AI-powered career recommendation systems.

---

## Repository Structure

```
job-ontology/
│
├── job-ontology.ttl          # Main ontology file (OWL 2 DL, Turtle serialisation)
├── ORSD_JobOntology.docx     # Ontology Requirements Specification Document (ORSD)
├── README.md                 # This file
└── docs/
    └── design-decisions.md   # Documented design rationale (see below)
```

---

## Core Concepts

### Classes

| Class | Description |
|---|---|
| `Person` | Any human participant in the recruitment process |
| `Candidate` | A job seeker (subclass of Person, disjoint with Recruiter) |
| `Recruiter` | An HR professional managing job postings (subclass of Person) |
| `Job` | A specific employment position offered by a company |
| `Company` | An organisation that posts job openings |
| `Skill` | A competency required by a job or possessed by a candidate |
| `TechnicalSkill` | A programming, engineering, or tool-related skill (subclass of Skill) |
| `SoftSkill` | An interpersonal or communication ability (subclass of Skill) |
| `ExperienceLevel` | A seniority classification (Junior / Mid / Senior) |
| `JobCategory` | A broad occupational grouping (e.g., Data Science) |

### Object Properties

| Property | Domain | Range | Description |
|---|---|---|---|
| `hasSkill` | Candidate | Skill | Skill possessed by a candidate |
| `isSkillOf` | Skill | Candidate | Inverse of `hasSkill` |
| `requiresSkill` | Job | Skill | Skill required by a job posting |
| `offeredBy` | Job | Company | Company that offers the job |
| `offersJob` | Company | Job | Inverse of `offeredBy` |
| `appliesTo` | Candidate | Job | Job a candidate has applied to |
| `recommendedFor` | Job | Candidate | Candidate recommended for a job |
| `hasExperienceLevel` | Candidate | ExperienceLevel | Candidate's seniority level |
| `requiresExperienceLevel` | Job | ExperienceLevel | Minimum seniority required |
| `belongsToCategory` | Job | JobCategory | Occupational category of a job |

### Datatype Properties

| Property | Domain | Range |
|---|---|---|
| `name` | Person | xsd:string |
| `email` | Person | xsd:string |
| `jobTitle` | Job | xsd:string |
| `jobDescription` | Job | xsd:string |
| `companyName` | Company | xsd:string |
| `location` | Company | xsd:string |
| `yearsOfExperience` | Candidate | xsd:int |

---

## Competency Questions (CQs)

The ontology is designed to answer the following competency questions:

| ID | Question |
|---|---|
| CQ1 | What skills does a given candidate possess? |
| CQ2 | What skills are required for a given job position? |
| CQ3 | Which candidates have **all** the skills required by a given job? |
| CQ4 | Which jobs match the skills of a given candidate? |
| CQ5 | What is the experience level of a given candidate? |
| CQ6 | What experience level does a given job require? |
| CQ7 | Which company offers a given job? |
| CQ8 | Which jobs does a given company offer? |
| CQ9 | Has a given candidate applied for a given job? |
| CQ10 | Which candidates are recommended for a given job? |
| CQ11 | What skills is a candidate **missing** for a target job (skill gap)? |
| CQ12 | How many years of experience does a candidate have? |

---

## Example SPARQL Queries

### CQ4 — Find jobs matching Naciye's skills

```sparql
PREFIX : <http://www.example.org/jobontology#>

SELECT ?job ?title WHERE {
  :Naciye :hasSkill ?skill .
  ?job :requiresSkill ?skill .
  ?job :jobTitle ?title .
}
GROUP BY ?job ?title
HAVING (COUNT(?skill) = (
  SELECT (COUNT(?rs) AS ?reqCount) WHERE {
    ?job :requiresSkill ?rs .
  }
))
```

### CQ11 — Skill gap for Naciye vs. DataScientistJob

```sparql
PREFIX : <http://www.example.org/jobontology#>

SELECT ?missingSkill WHERE {
  :DataScientistJob :requiresSkill ?missingSkill .
  FILTER NOT EXISTS { :Naciye :hasSkill ?missingSkill }
}
```

---

## Named Individuals (Example Instances)

### Skills
`Python`, `MachineLearning`, `DataAnalysis`, `SQL`, `DeepLearning`, `DataVisualization`, `CommunicationSkill`, `ProblemSolving`

### Experience Levels
`Junior` (0–2 yrs) · `Mid` (3–5 yrs) · `Senior` (6+ yrs)

### Companies
`TechCorp` · `DataHub Analytics`

### Jobs
`DataScientistJob` · `JuniorDataAnalystJob` · `MLEngineerJob`

### Candidates
| Name | Skills | Level | Yrs Exp |
|---|---|---|---|
| Naciye Nur Akkuş | Python, DataAnalysis, DataVisualization | Junior | 1 |
| Berat Yılmaz | Python, MachineLearning, SQL | Mid | 3 |
| Zeynep Arslan | Python, MachineLearning, DeepLearning, SQL | Senior | 7 |

---

## How to Use

### Load in Protégé
1. Open **Protégé 5.x**
2. `File → Open` → select `job-ontology.ttl`
3. Run the **HermiT** or **Pellet** reasoner to check consistency and infer new facts.

### Query with Apache Jena
```bash
# Install Apache Jena
# Run a SPARQL query
sparql --data job-ontology.ttl --query query.sparql
```

### Validate with ROBOT
```bash
robot verify --input job-ontology.ttl --queries sparql/cq1.sparql
```

---

## Future Work

- [ ] Add **skill proficiency levels** (Beginner / Intermediate / Expert) as a property on `hasSkill`
- [ ] Include **certifications** as a separate class linked to Candidates and Skills
- [ ] Integrate **real job market data** via the LinkedIn or Indeed APIs
- [ ] Add **SWRL rules** or **SHACL constraints** for automated recommendation reasoning
- [ ] Align with external vocabularies: [schema.org/JobPosting](https://schema.org/JobPosting), [ESCO](https://esco.ec.europa.eu/), [FOAF](http://xmlns.com/foaf/0.1/)
- [ ] Add a **machine learning recommendation layer** consuming the ontology as a knowledge graph

---

## Design Decisions

| Decision | Rationale |
|---|---|
| `Candidate` disjoint with `Recruiter` | Enforces semantic correctness; a person cannot simultaneously be both in the same context |
| Inverse properties (`hasSkill`/`isSkillOf`, `offeredBy`/`offersJob`) | Enables bidirectional SPARQL traversal without redundant assertions |
| `TechnicalSkill` and `SoftSkill` as subclasses of `Skill` | Allows finer-grained queries while keeping the class hierarchy shallow |
| `recommendedFor` as explicit assertion | In v1.0, recommendations are explicitly stated; in future versions they will be derived via SWRL or SPARQL CONSTRUCT rules |
| OWL 2 DL (not Full) | Guarantees decidability for automated reasoning |

---

## License

This ontology is developed as an academic project. No commercial use without permission from the authors.

---

## 👥 Contributors

- **Naciye Nur Akkuş** — Ontology Design, Instance Data, Documentation  
- **Elif Sude Yılmaz** — Ontology Design, SPARQL Queries, GitHub Setup
