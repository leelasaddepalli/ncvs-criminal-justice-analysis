# Gender and Violent Personal Crime: An Analysis of the NCVS 2023
**Washington and Lee University | CBSC 309: Statistical Methods**  
**Authors:** Leela Addepalli and Faith Sconing

---

Research report linked [here][https://github.com/leelasaddepalli/ncvs-criminal-justice-analysis/blob/main/309%20Final%20Project.pdf]
Slide deck linked [here][https://github.com/leelasaddepalli/ncvs-criminal-justice-analysis/blob/main/309%20Final%20Project%20Presentation.pdf]

## Overview

This project analyzes the 2023 **National Crime Victimization Survey (NCVS)** — a federal survey conducted annually by the Bureau of Justice Statistics — to examine how gender relates to the likelihood and intensity of violent personal crime victimization in the United States.

Working with over **275,000 observations** across four linked datasets, this project involved large-scale data extraction, variable construction from a 1,261-variable codebook, composite score computation, and multiple statistical modeling approaches including multiple regression, exploratory factor analysis (EFA), and Cronbach's alpha reliability testing.

---

## Research Questions

1. **How does gender affect the likelihood of being a victim of violent personal crime?**
2. **What is the correlation between attempts of violent crime and actual occurrence of violent crime?**

---

## Dataset

**Source:** National Crime Victimization Survey (NCVS) 2023, accessed via the National Archive of Criminal Justice Data (NACJD) / ICPSR (Collection 38962)

The NCVS file contains four linked datasets:

| Dataset | Key Variables | Observations |
|---|---|---|
| Address Record | Household address, year, sample/person ID | 275,343 |
| Household Record | Address + land ownership, residents, physical attributes | 254,071 |
| Person Record | Demographics: sex, gender, race, education | 255,901 |
| Incident Record | Nature of incident, injuries, pre/post context | 9,324 |

This study joined the **Person Record** and **Incident Record** datasets for analysis.

> **Note:** Raw NCVS data files are not included in this repository as they are publicly available via [ICPSR](https://www.icpsr.umich.edu/web/NACJD/studies/38962). The `.Rmd` file assumes local file paths consistent with the ICPSR download structure.

---

## Variable Construction

22 binary (yes/no) incident variables from the NCVS codebook were manually reviewed and combined into 6 composite continuous variables, grouped into **attempted** and **actual** violent crime categories:

**Attempted Violent Crime**
- `att_agg_batt_avg` — Attempted Aggravated Battery (5 items: weapon present, knife, other weapon, thrown object, threat to kill)
- `att_batt_avg` — Attempted Battery (3 items: hit/grab/jump, other, followed/surrounded)
- `att_SA_avg` — Attempted Sexual Crime (3 items: threat of rape, threat of sexual assault, sexual contact with force)

**Actual Violent Crime**
- `agg_bat_avg` — Aggravated Battery (6 items: shot, stabbed, hit by object, thrown object, other weapon, threat and attack)
- `bat_avg` — Battery (2 items: hit/slapped, grabbed/tripped)
- `SA_avg` — Sexual Crime (3 items: rape, sexual assault, tried to rape)

Each composite score represents the mean of its component binary variables, creating a continuous intensity measure ranging from 0 to 1.

---

## Methods

- **Data wrangling:** `tidyverse` — joining person and incident records, variable selection from 1,261-variable codebook, recoding binary items, computing composite scores, handling residual/missing data
- **Descriptive visualization:** `ggplot2`, `yarrr` (pirateplot) — bar charts and distribution plots for both expert and lay audiences
- **Effect size:** `effectsize` — probability of superiority statistic with 95% CIs for gender comparisons
- **Multiple regression:** `lm()`, `lm.beta` — modeling actual violent crime as a function of attempted violent crime composites
- **Exploratory Factor Analysis:** `psych` — VSS, scree plot, 1- and 2-factor oblimin models, RMSEA comparison
- **Reliability:** `psych::alpha()` — Cronbach's alpha for internal consistency of composite variables

---

## Key Findings

**Gender and Violent Crime**
- Total incident counts were nearly identical across male and female respondents — gender alone did not reliably predict overall victimization frequency
- Women were **reliably more likely** to experience sexual assault than men (probability of superiority = 0.35, 95% CI [0.32, 0.38]), meaning there is only a 35% chance a male experiences sexual assault at greater intensity than a female
- No statistically reliable gender difference was found for battery, aggravated battery, or their attempted equivalents

**Attempt vs. Actual Crime**
- Battery and aggravated battery showed small positive correlations with attempted crime composites (R² = .111 and .113 respectively), with attempted sexual assault as the strongest unique predictor
- Actual sexual assault was **negatively correlated** with all attempted crime predictors (R² = .036) — a finding with systemic implications for how sexual assault is charged and prosecuted relative to other violent crimes
- EFA was inconclusive: neither a 1- nor 2-factor model fit the data adequately (RMSEA = .212 and .231 respectively, vs. acceptable threshold of ≤.05)

**Reliability**
- Cronbach's alpha revealed that `att_SA_avg` needed reverse scoring — suggesting attempted sexual assault was coded as categorically distinct from attempted battery in the survey instrument
- Internal consistency improved substantially when `att_SA_avg` was dropped from the attempted crime composite (α = .48 → .75)

---

## Ethical Reflection

This project includes a full ethical reflection section in the `.Rmd` file covering:
- **Data extraction limitations:** Single-year snapshot (2023) vs. available 30-year longitudinal file; only 2 of 4 NCVS datasets used
- **Missing data:** ~200,000 observations dropped to handle NAs, with no demographic analysis of which respondents were excluded
- **Race:** Overrepresentation of white respondents; multi-race respondents collapsed to "Mixed" — analysis of race ultimately omitted to avoid invalid inferences
- **Gender coding:** NCVS encodes gender as binary sex assigned at birth; non-binary identities are not represented in the data
- **Researcher positionality:** Both researchers are educated women, which may have shaped variable selection and interpretive framing

---

## Repository Structure

```
ncvs-criminal-justice-analysis/
│
├── 309_Final_Project.Rmd           # Full analysis: data loading, variable
│                                   # construction, visualization, regression,
│                                   # EFA, reliability, conclusions
│
├── 309_Final_Project_Presentation  # Slide deck presented to class, showing
│   .pdf                            # expert and layperson visualizations
│                                   # side by side
│
└── README.md
```

---

## Tools & Packages

| Package | Use |
|---|---|
| `tidyverse` | Data wrangling, joins, variable recoding |
| `ggplot2` | Bar charts and layered visualizations |
| `yarrr` | Pirateplot distribution visualizations |
| `psych` | EFA, scree plot, VSS, Cronbach's alpha |
| `effectsize` | Probability of superiority statistic |
| `lm.beta` | Standardized regression coefficients |

---

## Data Access

The NCVS 2023 data used in this project is publicly available through ICPSR:  
[https://www.icpsr.umich.edu/web/NACJD/studies/38962](https://www.icpsr.umich.edu/web/NACJD/studies/38962)

Download the DS0003 (Person Record) and DS0004 (Incident Record) files and update the `load()` paths in the `.Rmd` file to match your local directory structure before running.
