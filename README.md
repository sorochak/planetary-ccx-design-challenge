# planetary-ccx-design-challenge

## Summary

The Carbon Credit eXchange (CCX) currently handles project verification through spreadsheets and email, as described in the design challenge. This leads to difficulty tracking verification status internally and insufficient transparency for buyers. As CCX scales to serve more suppliers, buyers, and verifiers, current processes create operational bottlenecks and limit transparency. A system-level solution is needed to support structured workflows, internal coordination, and external transparency.

---

## Goals

- Replace ad-hoc verification workflows (spreadsheets, email) with structured, trackable systems.

- Provide internal staff with visibility into project verification and validation stages.

- Equip verifiers with tools to review and submit standardized documentation.

- Expose verification details to buyers to improve credit transparency and confidence.

- Ensure scalable coordination as the number of users and projects increases.

- Establish an auditable system aligned with scientific and operational integrity.

---

## Stakeholders

| Role           | Primary Need                                                               |
| -------------- | -------------------------------------------------------------------------- |
| Internal Staff | Track project status, coordinate with verifiers and suppliers efficiently  |
| Suppliers      | Submit projects and understand verification progress                       |
| Verifiers      | Access project data, submit assessments in a structured format             |
| Buyers         | Understand how credits were verified to assess quality and trustworthiness |
| Public         | Gain confidence in CCX through accessible verification and project detail  |

---

## Design Areas

### 1. Transparency & Information Security Between CCX & External Customers

#### Assumptions

- Verification data includes both sensitive and public-facing elements.
- Buyers need verifiable summaries and validation status, not full access to raw data and documents.
- Trust is built through transparency in process, not unrestricted data access.

#### Key Questions

- Which documents or data fields should remain confidential?
- What information must be exposed to support buyer confidence?
- How can audit trails be made visible without compromising sensitive data?
- What rules determine when and how verification data is made public?

#### Design Suggestions

- Role-based access controls can be used to restrict visibility based on user roles.
- Internal users (e.g., staff, verifiers) would access full project and verification records.
- External users (e.g., buyers, public) would receive high-level summaries, verification outcomes, and non-sensitive metadata.
- Proprietary documents, raw datasets, and personally identifiable information should remain protected.
- When needed, summaries could support transparency without disclosing sensitive content.

---

### 2. Data Storage, Processing, Display, and Performance

#### Assumptions

- The system will handle a mix of structured data, semi-structured/unstructured data, and potentially geospatial data.
- Performance expectations are moderate (thousands of users/day).
- Relational databases are a good fit for some data, but not necessarily for everything.
- Not every performance improvement (e.g. CDN caching) is worth the complexity if page views are moderate.
- The public registry must load quickly and remain available under load.
- Cloud infrastructure and object storage (e.g., S3) can be used for some data.

#### Key Questions

- What are the data types we need to support, and how should each be stored and indexed?
- Are we working with structured data, semi-structured data, or non relational data?
- What are the most common or performance-critical user actions in the system?
- How do we handle large data files?
- What are the primary ways users will interact with and view data (tables, charts, maps), and how should that influence storage and processing choices?

#### Design Suggestions

- CCX should adopt a cloud-native storage architecture.
- Structured data should be stored in a relational database (e.g., PostgreSQL), optimized with indexing and pagination for fast public queries.
- Semi-structured and unstructured data should be stored in object storage (e.g., S3), with metadata and access links tracked in the relational layer.
- Public-facing pages can default to filtered or summarized views (e.g., by geography or recent dates) to reduce payload size and improve perceived performance.
- The platform should include basic observability tools: monitoring dashboards tracking request latency and P95 load times.

---

### 3. Data Integration Between Verifiers & CDR Pathways

#### Assumptions

- Data quality, format, and granularity will vary across verifiers, suppliers, and CDR methodologies.
- Some data might be received through manual uploads, others via API or automated pipelines.
- CCX requires visibility into ingestion outcomes to resolve issues quickly.
- Verifier-specific data formats may not align but must be mapped to common internal models.

#### Key Questions

- What minimum data schema must all verifiers and suppliers conform to, regardless of pathway?
- How do we handle inconsistent data from suppliers?
- How should the system detect, log, and respond to malformed or incomplete data submissions?
- What types of ingestion (real-time, batch, manual upload) need to be supported?
- What workflows should be triggered when ingestion fails or issues are flagged?

#### Design Suggestions

- Modular ETL pipelines with validation steps.
- Structured logging of all ingestion events and failures with alerting system.
- Interface or dashboard for reviewing and handling ingestion issues.

---

## Use of Generative AI

In compliance with Planetary’s AI policy, here are the access links to two ChatGPT conversations used to contribute to the design content:

https://chatgpt.com/share/685e0a46-f290-8000-9b98-3be33f1cd6d6<br>
https://chatgpt.com/share/685e0e47-ca4c-8000-ba16-24e636c98d23

---

## Appendix

### Banana Haiku

What fruit do you like<br>
Bananas are my favourite<br>
Robots don't make art

## Thanks

Thank you for the opportunity to work on this design challenge. I look forward to discussing these ideas further in the interview.

— Austen Sorochak
