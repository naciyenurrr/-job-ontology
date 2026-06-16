# Changelog — Job & Career Recommendation Ontology

All notable changes to this project are documented in this file.  
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [2.0] — 2026-05-12

### Added — Classes
- `:SkillProficiency` — Beginner / Intermediate / Expert mastery levels
- `:SkillAssessment` — Reification node linking Candidate + Skill + Proficiency
- `:Certification` — Formal credential (e.g. Google ML Engineer cert)
- `:Sector` — Industry vertical (TechSector, FinTech, HealthTech, ECommerce)

### Added — Object Properties
- `:hasSkillAssessment` (Candidate → SkillAssessment)
- `:assessedSkill` (SkillAssessment → Skill)
- `:hasProficiency` (SkillAssessment → SkillProficiency)
- `:holdsCertification` (Candidate → Certification)
- `:certifiesSkill` (Certification → Skill)
- `:requiresCertification` (Job → Certification)
- `:operatesInSector` (Company → Sector)
- `:targetsSector` (Job → Sector)

### Added — Datatype Properties
- `:certificationName`, `:issuingBody`, `:issueDate`, `:expiryDate` on Certification
- `:salary` on Job
- `:remote` (xsd:boolean) on Job

### Added — Named Individuals
- Skills: `:NLP`, `:CloudComputing`, `:Spark`, `:Tableau`, `:Teamwork`
- Proficiency levels: `:Beginner`, `:Intermediate`, `:Expert`
- Sectors: `:TechSector`, `:FinTech`, `:HealthTech`, `:ECommerce`
- Certifications: `:GoogleMLCert`, `:AWSDACert`, `:TableauDesktopCert`
- Companies: `:FinAI`
- Jobs: `:DataEngineerJob`, `:NLPResearcherJob`
- Candidates: `:Cem` (Cem Kaya), `:Selin` (Selin Demir)
- SkillAssessments: 6 new assessment individuals

### Added — External Alignments
- `schema:` — Job → schema:JobPosting, Person → schema:Person
- `skos:exactMatch` annotations on Skill individuals (ESCO alignment)
- `foaf:` annotations on Person individuals
- Dublin Core metadata on ontology header

### Added — Competency Questions
- CQ13: Skill proficiency level of a candidate
- CQ14: Certifications held by a candidate
- CQ15: Jobs requiring a specific certification
- CQ16: Candidates meeting all certification requirements of a job

### Added — Tooling
- SHACL shapes file (`shacl/shapes.ttl`) for instance validation
- SPARQL query files for all 16 CQs (`sparql/`)
- WIDOCO HTML documentation (`index.html`) deployed via GitHub Pages
- LLM extraction pipeline notes in `data/`

### Changed
- Ontology version IRI updated to `jobontology/2.0`
- `owl:priorVersion` pointing to `jobontology/1.0` added
- `dc:date` updated to `2026-05-12`
- `dc:description` extended to mention V2 features
- `:JuniorDataAnalystJob` now also requires `:Tableau` skill and `:TableauDesktopCert`
- `:MLEngineerJob` now also requires `:CloudComputing` and `:GoogleMLCert`
- Candidate `:Naciye` upgraded with SQL skill, Tableau cert, and SkillAssessments
- Candidate `:Zeynep` name corrected (was "Elif Sude Yılmaz" in v1 — now "Zeynep Arslan")

### Fixed
- Recommendation assertion `:MLEngineerJob :recommendedFor :Elif` corrected to `:Zeynep`

---

## [1.0] — 2026-04-28

### Added
- Core classes: Person, Candidate, Recruiter, Job, Company, Skill,
  TechnicalSkill, SoftSkill, ExperienceLevel, JobCategory
- Object properties: hasSkill, isSkillOf, requiresSkill, offeredBy,
  offersJob, appliesTo, recommendedFor, hasExperienceLevel,
  requiresExperienceLevel, belongsToCategory
- Datatype properties: name, email, jobTitle, jobDescription,
  companyName, location, yearsOfExperience
- Named individuals: Python, MachineLearning, DataAnalysis, SQL,
  DeepLearning, DataVisualization, CommunicationSkill, ProblemSolving
- Experience levels: Junior (0–2 yrs), Mid (3–5 yrs), Senior (6+ yrs)
- Companies: TechCorp, DataHub
- Jobs: DataScientistJob, JuniorDataAnalystJob, MLEngineerJob
- Candidates: Naciye, Berat, Zeynep
- Competency questions CQ1–CQ12
- OWL disjointness: Candidate ⊥ Recruiter
- Inverse properties: hasSkill/isSkillOf, offeredBy/offersJob

---

*Maintained by Naciye Nur Akkuş & Elif Sude Yılmaz — Manisa Celal Bayar Üniversitesi, Spring 2026*
