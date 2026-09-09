# Resources — Hierarchy & Selection System

> How resources enter, live, and get demoted in this repository. The per-domain tables live in [`docs/05-resource-map.md`](../docs/05-resource-map.md) and each phase README; this file is the constitution.

---

## 1. The Four Tiers

**Tier 1 — Primary / Authoritative.** Regulators (Fed, OCC, CFPB, FinCEN, EU institutions, UK PSR/FCA), central banks & BIS, standards bodies (FATF, Wolfsberg, ISO, BCBS), peer-reviewed papers, official technology documentation (Kafka, Flink, Feast, QuantLib, Hugging Face...). *These are the source of truth. When a blog contradicts a regulator, the blog is wrong.*

**Tier 2 — Technical education.** University courses (MIT OCW, NYU, Stanford public lectures), rigorous textbooks (Hull, Mishkin, Baesens, Kleppmann, Huyen), expert lecture series. *The depth layer — where understanding is actually built.*

**Tier 3 — Practitioner.** Engineering blogs with real production detail (Stripe, Adyen, Revolut, Capital One, Netflix), competition write-ups with methodology (Kaggle, M5), OSS project docs/blogs, conference talks with substance. *The reality layer — what production actually looks like.*

**Tier 4 — Supplementary.** Video explainers, tutorials, community threads, aggregators. *Motivation and reps. Never citable as authority in your notes.*

## 2. Admission Test (all six, or it doesn't enter)

1. **Authority:** identifiable, accountable author/institution.
2. **Depth:** contains math, code, decisions, or data — not just assertions.
3. **Recency check:** date known; "still true?" answered (regulations verified, vendor claims flagged).
4. **Actionability:** changes what you will build or how you will evaluate.
5. **Non-redundancy:** does not duplicate an already-admitted resource for the same niche.
6. **Cost-of-error:** for factual claims, what happens if it's wrong? High-blast-radius claims need Tier 1 backing.

## 3. Priority Labels (used in every phase table)

| Label | Meaning |
|---|---|
| **Essential** | The phase cannot be passed without engaging this |
| **Recommended** | Strong default; skip with a stated reason |
| **Optional** | Useful under specific goals (interviews, specialization) |
| **Reference** | Lookup, not linear consumption |

## 4. Consumption Protocol

```text
Pick 1 Tier-1 + 1 Tier-2 + 1 Tier-3 per topic (the "1-1-1 rule")
→ read/implement with the learning loop
→ 10-line synthesis note in your own words
→ log one line: what it delivered, what it skipped, priority change?
→ demote/promote in the phase table (CONTRIBUTING §5)
```

Budget per week: ~2-3 h reading *while building*. If reading hours exceed building hours two weeks running, invoke LEARNING.md's anti-completionism rule #3.

## 5. The Standing Resource Registry (infrastructure you'll live in)

| Kind | Names |
|---|---|
| Regulatory/primary | federalreserve.gov (SR letters), bis.org, cfpb.gov, fca.org.uk/psr.org.uk, fatf-gafi.org, ofac.treasury.gov, ffiec.gov, fincen.gov, eu AI Act/DORA texts, nist.gov AI RMF |
| Data portals | fred.stlouisfed.org, ecb.europa.eu data, data.worldbank.org, sec.gov (EDGAR), consumerfinance.gov data, gleif.org, nasdaqdatalink.com |
| OSS docs | kafka.apache.org, flink.apache.org, feast.dev, getdbt.com, duckdb.org, pola.rs, mlflow.org, evidentlyai docs, optbinning docs, huggingface.co docs, llamaindex/haystack, quantlib.org, nixtla docs |
| Practitioner blogs | Stripe blog, Adyen tech, Revolut engineering, Confluent blog, Chainalysis research, Kaggle write-ups, M5 reports |
| Courses (free-first) | MIT OCW 15.401/18.S096, Yale Shiller (Coursera), FPP3, DataTalksClub (DE/MLOps/LLM) Zoomcamps, HF NLP course, QuantConnect Bootcamp |

## 6. Quarterly Resource Review (15 minutes)

1. Verify 5 regulatory claims you've been quoting (dates change — e.g., AI Act phase-in status).
2. Check one "current practice" claim per Stage V area (agents, RAG, TSFMs).
3. Demote anything that under-delivered; promote what you kept returning to.
4. Prune the registry: a resource you haven't touched in a year and won't this year gets archived (git history remembers).
