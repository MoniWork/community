# U.S. Education Sector Email Security Intelligence Dataset

**Data Product Brief for Education Researchers**

*Lokentra Research Division -- March 2026*

---

## The Dataset at a Glance

### Full Lokentra ESI Registry

| Metric | Value |
|--------|-------|
| **Total domains DNS-profiled** | **406,680** |
| Domains alive (resolving) | 391,984 (96.4%) |
| Public sector entities | 69,892 (35,862 unique domains) |
| For-profit businesses (SAM.gov) | 273,713 |
| Nonprofits (IRS BMF) | 84,272 |
| States covered | 50 + District of Columbia |
| DNS record types per domain | 7 (A, CNAME, MX, NS, SPF, DMARC, SOA) |
| Data collection period | March 2026 |
| Entity schema fields | 40 per entity |

### Education Segment (This Brief)

| Metric | Value |
|--------|-------|
| K-12 school districts and charter LEAs | 22,741 |
| Higher education institutions | 3,246 |
| **Total education entities** | **25,987** |
| K-12 with website / domain | 18,274 |
| K-12 with phone number | 19,281 (100% of CCD-matched) |
| K-12 with physical address | 19,281 (100% of CCD-matched) |
| K-12 with grade span | 19,281 (100% of CCD-matched) |
| K-12 with NCES LEAID (join key) | 19,281 |
| K-12 data freshness | **2024-2025 school year** |

---

## What Makes This Dataset Unique

### 1. No comparable dataset exists

The U.S. Census Bureau's Census of Governments (2022) counts government entities but does not provide internet domains, email provider classification, or DNS security posture. NCES CCD and IPEDS provide enrollment and finance data but no technology infrastructure signals. **This is the only dataset that links every U.S. K-12 district and accredited institution to its full DNS profile, email provider, security gateway status, and authentication posture.**

### 2. Entity-attributed, not domain-sampled

Unlike commercial threat intelligence feeds that start from domain lists, this dataset starts from authoritative government rosters and works forward to DNS. Every record is attributed to a named entity with entity type, subtype, state, county, and source provenance. Researchers can join this data to NCES, Census, or SAIPE datasets by district name, state, and county.

### 3. Full DNS security stack, not just DMARC

Each domain is profiled across the complete email authentication stack:
- **SPF** (Sender Policy Framework) -- record presence, qualifier (`-all` vs `~all`)
- **DKIM** (DomainKeys Identified Mail) -- public key presence across common selectors
- **DMARC** -- policy level (`none` / `quarantine` / `reject`), reporting address
- **MX records** -- primary email provider classification
- **Email security gateway detection** -- proxy vs direct mail routing
- **Underlying provider inference** -- SPF-based detection of the real mailbox platform behind gateways

---

## Key Findings Education Researchers Can Build On

### Finding 1: K-12 is Google's stronghold -- a 4:1 ratio over Microsoft

| Segment | Google Workspace | Microsoft 365 | Ratio |
|---------|----------------:|---------------:|------:|
| K-12 school districts | 8,611 | 2,159 | 4.0 : 1 |
| Higher education | 579 | 1,680 | 1 : 2.9 |

K-12 districts overwhelmingly use Google Workspace, driven by Google Workspace for Education's free/discounted licensing. Higher education inverts this pattern, favoring Microsoft 365. **This is the first empirical quantification of this bifurcation at national scale.**

**Research angles**: cloud vendor lock-in in public education, procurement decision drivers, EdTech ecosystem effects, digital equity implications.

### Finding 2: Half of K-12 districts lack DMARC -- vulnerable to spoofing

| Protocol | K-12 Adoption | Higher Ed Adoption | National Average |
|----------|-------------:|-------------------:|-----------------:|
| MX records | 85.7% | 93.3% | 81.0% |
| SPF | 79.1% | 90.5% | 75.9% |
| DMARC (any) | 51.7% | 86.1% | 47.2% |
| DMARC reject | ~25% | ~55% | ~22.6% |

**48.3% of K-12 domains have no DMARC record at all.** These districts can be impersonated via email spoofing -- a direct phishing vector against parents, staff, and students. Meanwhile, higher education institutions achieve 86.1% DMARC adoption, a 34.4 percentage-point lead over K-12.

**Research angles**: institutional capacity and cybersecurity outcomes, federal mandate gaps (BOD 18-01 covers federal agencies only), K-12 cybersecurity readiness, phishing risk to minors.

### Finding 3: K-12 districts rarely deploy email security gateways

| Entity Type | Domains with MX | Using Email Security Gateway | Adoption Rate |
|-------------|----------------:|-----------------------------:|-------------:|
| Higher education | 2,767 | 564 | 20.4% |
| K-12 | 12,607 | 1,033 | 8.2% |

Only 8.2% of K-12 districts route mail through a third-party email security gateway (Barracuda, Proofpoint, Mimecast, etc.), versus 20.4% for higher education. Districts that do use gateways show dramatically better security posture:

| Cohort | SPF | DMARC |
|--------|----:|------:|
| Gateway-proxied domains | 97.3% | 75.7% |
| Non-proxied domains | 89.2% | 53.0% |
| **Uplift** | **+8.1pp** | **+22.7pp** |

**Research angles**: IT staffing and budget constraints in K-12, vendor ecosystem effects on security outcomes, policy interventions to improve K-12 email security.

### Finding 4: State-by-state variation reveals policy effects

The dataset covers all 50 states + DC with per-state breakdowns. Example variation in K-12 DMARC adoption:

- States with centralized IT governance or cybersecurity mandates show higher adoption
- States with fragmented district structures show lower adoption
- Rural vs urban district size correlates with security posture

**Research angles**: policy effectiveness studies, cross-state comparative analysis, rural digital divide, state cybersecurity mandate impact assessment.

### Finding 5: Provider choice predicts security -- independent of budget

| Provider Category | SPF | DMARC | SPF-DMARC Gap |
|-------------------|----:|------:|--------------:|
| Email Security Proxy | 97.3% | 75.7% | 21.6pp |
| Enterprise Cloud (Google/M365) | 93.2% | 60.4% | 32.8pp |
| Budget Hosting (GoDaddy, IONOS) | 51.3% | 23.8% | 27.5pp |

Districts using budget hosting providers (GoDaddy email: 7.3% SPF) are effectively unprotected. The vendor ecosystem a district selects determines its security floor.

**Research angles**: technology procurement and security outcomes, vendor accountability, minimum viable security standards for education.

---

## Dataset Schema (Education-Relevant Fields)

| Field | Type | Description |
|-------|------|-------------|
| `entity_name` | string | Official district or institution name |
| `entity_type` | enum | `k12` or `higher_ed` |
| `entity_subtype` | string | `school_district`, `charter_org`, `community_college`, `university`, etc. |
| `state` | string | Two-letter USPS code |
| `county` | string | County name |
| `primary_domain` | string | Apex domain (e.g., `springisd.org`) |
| `mx_provider` | string | Classified email provider |
| `has_spf` | boolean | SPF record present |
| `spf_qualifier` | string | `-all`, `~all`, `?all` |
| `has_dkim` | boolean | DKIM public key found |
| `has_dmarc` | boolean | DMARC record present |
| `dmarc_policy` | string | `none`, `quarantine`, `reject` |
| `email_proxy` | string | Security gateway service name (if any) |
| `underlying_provider` | string | Real mailbox platform behind gateway |
| `dns_score` | integer | 0-100 composite security score |
| `grade` | string | A / B / C / D / F |
| `source_name` | string | Authoritative collection source |
| `source_url` | string | Exact URL of source data |
| `collected_at` | ISO 8601 | Collection timestamp |
| `nces_leaid` | string | NCES LEA ID (canonical join key to NCES finance/enrollment) |
| `phone` | string | District main phone number |
| `physical_address` | string | Physical street address |
| `mailing_address` | string | Mailing address |
| `grade_low` / `grade_high` | string | Grade span (e.g., PK to 12) |
| `operational_schools` | integer | Number of operational schools in district |
| `lea_type` | string | LEA type code (regular, charter, regional, etc.) |
| `charter_flag` | string | Charter status (NOTCHR, CHRTIDEAESEA, etc.) |

---

## Research Applications

### For Education Policy Researchers
- **Cybersecurity readiness assessment** -- Which states/districts are most vulnerable to email-based attacks?
- **Federal mandate gap analysis** -- BOD 18-01 covers federal agencies; what would a K-12 equivalent look like?
- **Digital equity** -- Do rural, underfunded districts have systematically worse email security?
- **State policy effectiveness** -- Do states with cybersecurity mandates show better K-12 adoption rates?

### For EdTech and Procurement Researchers
- **Cloud adoption patterns** -- Quantify the Google vs Microsoft split across education segments
- **Vendor lock-in analysis** -- How does the free Google Workspace for Education tier affect switching costs?
- **Email security gateway market** -- Which vendors serve education? What's the adoption curve?
- **Total Addressable Market (TAM)** -- 8,200+ K-12 districts without email security gateways

### For Cybersecurity Researchers
- **Phishing risk modeling** -- Map authentication gaps to phishing exposure at the district level
- **DMARC deployment barriers** -- Why do 48% of K-12 districts lack DMARC despite having SPF?
- **Provider-security causal analysis** -- Does Google Workspace's auto-provisioning of SPF explain the adoption gap?
- **Longitudinal tracking** -- Baseline for measuring future adoption progress

---

## ESI Scoring Rubric

Each domain is scored on a 100-point scale across three email authentication protocols. The rubric rewards enforcement-level configuration, not just presence.

### Sender Policy Framework (SPF) -- 30 points

| Configuration | Points | Rationale |
|---------------|-------:|-----------|
| SPF record with `-all` (hard fail) | 30 | Rejects unauthorized senders outright |
| SPF record with `~all` (soft fail) | 15 | Flags unauthorized senders but still delivers |
| SPF record with `?all` or `+all` | 5 | Present but effectively permissive |
| No SPF record | 0 | No sender validation |

### DomainKeys Identified Mail (DKIM) -- 30 points

| Configuration | Points | Rationale |
|---------------|-------:|-----------|
| DKIM public key published (common selectors checked) | 30 | Proves message integrity and sender authenticity |
| No DKIM key found | 0 | Messages cannot be cryptographically verified |

Selectors checked: `google`, `mail`, `selector1`, `selector2`, `s1`, `s2`, `k1` (covers Google Workspace + Microsoft 365 defaults).

### Domain-based Message Authentication, Reporting, and Conformance (DMARC) -- 40 points

| Configuration | Points | Rationale |
|---------------|-------:|-----------|
| `p=reject` (full enforcement) | 40 | Spoofed messages are rejected by receiving servers |
| `p=quarantine` (partial enforcement) | 20 | Spoofed messages routed to spam/junk |
| `p=none` (monitoring only) | 10 | DMARC record exists but no enforcement action taken |
| No DMARC record | 0 | Domain is fully spoofable |

DMARC receives the highest weight (40%) because it is the only protocol that directly prevents domain spoofing in the `From:` header -- the field end users actually see.

### Composite Score and Grade

| Grade | Score Range | Interpretation |
|-------|:----------:|----------------|
| **A** | 90 -- 100 | Full enforcement: SPF hard fail + DKIM + DMARC reject |
| **B** | 70 -- 89 | Strong posture with minor gaps (e.g., DMARC quarantine instead of reject) |
| **C** | 50 -- 69 | Partial protection: SPF present, DMARC at monitoring or absent |
| **D** | 30 -- 49 | Weak: missing DKIM or DMARC, SPF soft fail |
| **F** | 0 -- 29 | Minimal or no email authentication configured |

### What the grades mean for education entities

- **Grade A district**: Parents and staff cannot receive spoofed email from this domain. The district has fully configured all three protocols at enforcement level.
- **Grade C district** (the K-12 national average): SPF likely present, but no DMARC. An attacker can send email that appears to come from `superintendent@district.org` and it will land in inboxes.
- **Grade F district**: Essentially no email authentication. Any sender can impersonate any address at this domain.

### Beyond Education: Cross-Sector Benchmarking Available

The ESI scoring rubric is applied uniformly across our full entity registry -- not just education. Researchers who need broader context can access scored datasets for:

- **For-profit businesses** -- sourced from SAM.gov federal entity registrations, with domain resolution and email security scoring
- **Nonprofits** -- sourced from IRS Business Master File (BMF) exempt organization data, DNS-profiled and graded
- **Public sector** -- 69,892 entities across municipalities, counties, state agencies, special districts, and townships

This enables cross-sector comparisons: *How does K-12 email security compare to the nonprofit sector? Do SAM.gov-registered contractors outperform the school districts they serve?* Education-only purchasers can add sector packs later without re-licensing.

---

## Delivery Format

| Option | Description |
|--------|-------------|
| **CSV/Parquet** | Flat files, one row per entity, all DNS fields included |
| **SQLite database** | Pre-indexed by state, entity type, provider, grade |
| **API access** | RESTful query interface with filtering and aggregation |
| **Interactive dashboard** | Web-based explorer with state/district drill-down |

All deliveries include:
- Full methodology documentation (peer-review ready)
- Source provenance for every entity record
- Data dictionary with field definitions and enum values
- Reproducibility scripts (Python) for DNS re-scanning

---

## Provenance and Methodology

### Data Sources (Education Entities)

| Source | Coverage | Method |
|--------|----------|--------|
| NCES Common Core of Data (CCD) | K-12 districts, all 50 states | Federal API |
| NCES IPEDS | Higher education, all 50 states | Federal API |
| State Departments of Education | K-12 districts, charter orgs | HTML scrape, CSV, API (per state) |
| State Higher Education Coordinating Boards | Colleges, universities | HTML scrape, API (per state) |
| U.S. Census Bureau Gazetteer | Entity coordinates, county attribution | Federal dataset |
| SAM.gov Entity Registration | Federal registration cross-reference | Federal API |

### DNS Profiling Pipeline

1. **Domain extraction** -- Parsed from entity website fields; apex domain isolated
2. **MX resolution** -- 50-thread parallel DNS resolution via `dnspython`
3. **Provider classification** -- MX hostname pattern matching (Google, M365, Barracuda, etc.)
4. **SPF/DMARC/DKIM resolution** -- Full TXT record parsing
5. **Gateway detection** -- MX vs SPF divergence analysis to identify proxy configurations
6. **Scoring** -- 100-point rubric: SPF (30), DKIM (30), DMARC (40)
7. **Deduplication** -- SHA-1 keyed on `normalized_name | entity_type | county`

### Quality Metrics

| Metric | Value |
|--------|-------|
| Domain resolution success rate | 98.8% |
| Entity deduplication rate | <2% collision rate |
| Source attribution | 100% (every record traced to source URL) |
| Collection scripts | Open-source Python, fully reproducible |

---

## Competitive Positioning

| Capability | This Dataset | NCES CCD/IPEDS | Commercial Threat Intel | Censys/Shodan |
|------------|:---:|:---:|:---:|:---:|
| Entity-attributed (named districts) | Yes | Yes | No | No |
| All 50 states + DC | Yes | Yes | Partial | N/A |
| Email provider classification | Yes | No | No | Partial |
| DMARC/SPF/DKIM posture | Yes | No | Yes (domains only) | No |
| Email gateway detection | Yes | No | Partial | No |
| Underlying provider inference | Yes | No | No | No |
| K-12 + Higher Ed combined | Yes | Separate DBs | No | No |
| Source-attributed and reproducible | Yes | Yes | No | No |
| Education-sector benchmarking | Yes | No | No | No |

---

## Pricing Tiers

| Tier | Scope | Suggested Use |
|------|-------|---------------|
| **State Pack** | Single state, all education entities | Regional studies, state policy analysis |
| **K-12 National** | All 22,741 K-12 districts + charters, 50 states | National K-12 cybersecurity research |
| **Higher Ed National** | All 3,246 institutions, 50 states | Higher ed technology adoption studies |
| **Full Education** | K-12 + Higher Ed combined | Cross-segment comparative research |
| **Full Public Sector** | All 69,892 entities (all types) | Comprehensive government technology research |
| **API + Updates** | Quarterly re-scan, API access | Longitudinal studies, dashboards |

*Contact for pricing: research@monitorworkspace.com*

---

## Sample Outputs

### Sample: State-Level K-12 DMARC Adoption Heatmap

Available as interactive visualization -- 50-state choropleth showing DMARC adoption rate by state for K-12 districts.

### Sample: Provider Market Share by Education Segment

Available as publication-ready charts (SVG/PNG) showing Google vs Microsoft split across K-12 and higher education.

---

## Contact

**Lokentra Research Division**
research@monitorworkspace.com
https://www.monitorworkspace.com/scorecard

*Free scorecard demo: Enter any education domain at the URL above to see a sample DNS security report.*

---

*This document and the underlying dataset are products of the Lokentra U.S. Email Security Index (ESI). Methodology papers available on request. All data derived from publicly accessible DNS records and government-published registries.*
