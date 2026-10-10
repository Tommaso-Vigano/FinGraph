# FinGraph

## Understanding the Dataset

FinGraph uses the [U.S. Securities and Exchange Commission (SEC) Form 13F Data Sets](https://www.sec.gov/data-research/sec-markets-data/form-13f-data-sets).

The downloaded dataset contains several files, but for our analysis, we focus on three main **Tab-Separated Values (TSV)** files.

### Dataset Structure

```text
data/
├── COVERPAGE.tsv
├── INFOTABLE.tsv
└── SUBMISSION.tsv
```

Each file contains different information about institutional investment managers, their SEC filings, and their reported securities holdings.

The following sections describe the attributes we retain from each file.

---

### 📄 1. COVERPAGE.tsv

Contains information about the investment manager submitting the filing, including amendments.

| Attribute | Description |
|:---|:---|
| `ACCESSION_NUMBER` | Connects the three datasets. |
| `FILINGMANAGER_NAME` | Human-readable name of the investment manager. |
| `ISAMENDMENT` | Indicates whether the filing is an [amendment](https://www.lexology.com/library/detail.aspx?g=5922b755-dc0f-45b6-b44d-1226d6f48343). |
| `AMENDMENTTYPE` | Specifies whether the amendment is a restatement or adds new holdings entries. |

---

### 📄 2. SUBMISSION.tsv

Contains metadata about each SEC filing, including the reporting entity, submission type, and relevant dates.

| Attribute | Description |
|:---|:---|
| `ACCESSION_NUMBER` | Connects the three datasets. |
| `CIK` | **Central Index Key:** A unique identification number assigned by the SEC to identify filing managers. |
| `PERIODOFREPORT` | Identifies the date of the portfolio snapshot. |
| `FILING_DATE` | Indicates when the information became public. |
| `SUBMISSIONTYPE` | Specifies the type of document submitted to the SEC by an investment manager. *(To be explored further in the blog article.)* |

---

### 📄 3. INFOTABLE.tsv

Contains the individual securities positions reported in each filing, including their identifiers, quantities, and market values.

| Attribute | Description |
|:---|:---|
| `ACCESSION_NUMBER` | Connects the three datasets. |
| `INFOTABLE_SK` | Within a given filing (identified by `ACCESSION_NUMBER`), uniquely identifies each row in `INFOTABLE.tsv`. |
| `CUSIP` | **Committee on Uniform Securities Identification Procedures:** A 9-character alphanumeric identifier used to identify financial securities, primarily in the United States and Canada. |
| `NAMEOFISSUER` | Human-readable name of the security's issuer. |
| `TITLEOFCLASS` | Distinguishes different types or classes of securities. |
| `VALUE` | Reported market value of the position. |
| `SSHPRNAMT` | Reported number of shares or principal amount. |
| `SSHPRNAMTTYPE` | Specifies whether the reported quantity represents shares or a principal amount. |
| `PUT_CALL` | Indicates whether the position is a PUT or CALL option. |

---

### 🔗 How the Datasets Are Connected

The three files share a common attribute: **`ACCESSION_NUMBER`**.

This identifier allows us to connect information about SEC filings, investment managers, and reported securities positions.

```text
                 ACCESSION_NUMBER
                         │
             ┌───────────┼───────────┐
             │           │           │
             ▼           ▼           ▼
        COVERPAGE    SUBMISSION   INFOTABLE
             │           │           │
             ▼           ▼           ▼
        Manager       Filing       Security
        Details      Metadata      Holdings
```

By combining these datasets, we can reconstruct the reported holdings of institutional investment managers and prepare the data for graph-based analysis using Neo4j.
