# YouthConnect After-School Program Dataset
## Demo Data for "From Data to Decisions" Session

---

## Dataset Overview

This synthetic dataset tells the story of **YouthConnect**, an after-school program operating in 5 Saskatchewan neighborhoods, seeking board approval for a 30% funding increase to expand from 200 to 300 participant slots.

### The Story
- **Started**: 2020 with 120 participants
- **Current**: 2024 with 200 participants at capacity
- **Waitlist**: Growing from 20 (2020) to 85+ (2024)
- **Impact**: 40+ percentage point improvement in college enrollment vs. regional averages
- **Cost efficiency**: Decreasing from $2,800 to $2,200 per participant
- **Ask**: $220,000 to serve 100 more youth

---

## Tables Structure

### 1. **locations.csv** (5 rows)
Our 5 program locations across Saskatchewan

| Field | Type | Description |
|-------|------|-------------|
| location_id | INT | Primary key (1-5) |
| location_name | VARCHAR(50) | "Riversdale Youth Hub", etc. |
| neighborhood | VARCHAR(50) | "Riversdale", "Cathedral", etc. |
| city | VARCHAR(50) | Saskatoon, Regina, Prince Albert, Moose Jaw |
| venue | VARCHAR(100) | Actual building name |
| median_income | INT | Neighborhood median income |
| youth_pop | INT | Youth population in neighborhood |
| capacity | INT | Max participants per location |

**Locations:**
1. Riversdale Youth Hub (Saskatoon) - 45 capacity
2. Cathedral Youth Centre (Regina) - 40 capacity
3. North Central Connect (Regina) - 50 capacity
4. East Hill Youth Program (Prince Albert) - 35 capacity
5. Downtown Moose Jaw Hub (Moose Jaw) - 30 capacity

---

### 2. **participants.csv** (~815 rows)
Individual participant records across 5 years

| Field | Type | Description |
|-------|------|-------------|
| participant_id | INT | Primary key |
| enroll_year | INT | 2020-2024 |
| location_id | INT | Foreign key to locations |
| age_group | VARCHAR(10) | "16-18" or "19-21" |
| attend_rate | DECIMAL(5,2) | Attendance percentage (45-100) |
| completed | VARCHAR(3) | "yes" or "no" |
| outcome | VARCHAR(20) | "college", "trade", "employed", "other" |
| cost_per_part | INT | Cost per participant that year |

**Growth trajectory:**
- 2020: 120 participants
- 2021: 145 participants
- 2022: 165 participants
- 2023: 185 participants
- 2024: 200 participants

---

### 2b. **participants_demographics.csv** (~815 rows)
Demographic details for participants (joins to participants via participant_id)

| Field | Type | Description |
|-------|------|-------------|
| participant_id | INT | Foreign key to participants |
| fileAs | NVARCHAR(102) | Display name (e.g., "Smith, Jane") |
| givenname | NVARCHAR(50) | First name |
| surname | NVARCHAR(50) | Last name |
| birthdate | DATE | Date of birth |
| gender | VARCHAR(20) | Gender identity |
| Address_1 | NVARCHAR(70) | Street address |
| city | NVARCHAR(35) | City |
| province | NVARCHAR(4) | Province code (SK, AB, MB) |
| postalCode | NVARCHAR(12) | Canadian postal code |

**Note:** This table simulates personally identifiable information (PII) for demonstration purposes. Names and birthdates are fictitious. Street addresses may or may not exist.

---

### 3. **waitlist.csv** (25 rows)
Unmet demand (5 locations × 5 years)

| Field | Type | Description |
|-------|------|-------------|
| year | INT | 2020-2024 |
| location_id | INT | Foreign key to locations |
| waitlist_cnt | INT | Number on waitlist |
| est_lost_part | INT | Estimated youth who couldn't participate |

**Key insight:** Waitlist grows significantly in lower-income neighborhoods (North Central, East Hill)

---

### 4. **regional_bench.csv** (50 rows)
Regional youth outcomes - "government statistics" (10 neighborhoods × 5 years)

| Field | Type | Description |
|-------|------|-------------|
| year | INT | 2020-2024 |
| neighborhood | VARCHAR(50) | Neighborhood name |
| city | VARCHAR(50) | City name |
| college_pct | DECIMAL(4,1) | % enrolled in college/university |
| unemp_pct | DECIMAL(4,1) | % unemployed |
| trade_pct | DECIMAL(4,1) | % enrolled in trade school |
| non_trade_pct | DECIMAL(4,1) | % employed (non-trade) |

**Includes 10 neighborhoods:**
- 5 WHERE we have programs (Riversdale, Cathedral, North Central, East Hill, Downtown Moose Jaw)
- 5 WHERE we DON'T (Nutana, Warehouse District, Midtown, Crescent Park, Downtown Swift Current)

**Key data:** Regional averages show ~40-42% college enrollment, ~12% unemployment

---

### 5. **program_outcomes.csv** (25 rows)
OUR participants' outcomes (5 locations × 5 years)

| Field | Type | Description |
|-------|------|-------------|
| year | INT | 2020-2024 |
| neighborhood | VARCHAR(50) | Matches locations |
| city | VARCHAR(50) | City name |
| college_pct | DECIMAL(4,1) | % OUR participants in college |
| unemp_pct | DECIMAL(4,1) | % OUR participants unemployed |
| trade_pct | DECIMAL(4,1) | % OUR participants in trades |
| non_trade_pct | DECIMAL(4,1) | % OUR participants employed (non-trade) |

**Key data:** Our participants show ~78-83% college enrollment, ~3-5% unemployment

---

## The Compelling Story in the Data

### Impact Comparison (2024 Riversdale example):
- **Regional college enrollment:** ~41%
- **Program college enrollment:** ~83%
- **IMPACT:** +42 percentage points

- **Regional unemployment:** ~12%
- **Program unemployment:** ~4%
- **IMPACT:** -8 percentage points

### Cost Efficiency:
- 2020: $2,800 per participant
- 2024: $2,200 per participant
- **Improvement:** 21% cost reduction through economies of scale

### Unmet Demand:
- 2020 waitlist: ~20 youth
- 2024 waitlist: ~85 youth
- **Growth:** 325% increase in unserved demand

### The Ask:
- 100 additional slots × $2,200 = **$220,000 investment**
- Serves 85 currently waitlisted + room for growth
- Proven ROI: 40+ point improvement in life outcomes

---

## Power Query Demo: Messy Version

**participants_messy.csv** (~150 rows for demo purposes)

Issues intentionally included:
- Column headers with spaces: "Participant ID", "Enroll Year"
- Blank rows scattered throughout
- Inconsistent date formats
- Numbers stored as text (quoted)
- Extra whitespace in text fields

Perfect for demonstrating Power Query's cleanup capabilities!

---

## SQL Scripts

### create_tables.sql
```sql
-- YouthConnect Database Schema

CREATE TABLE locations (
    location_id INT PRIMARY KEY,
    location_name VARCHAR(50) NOT NULL,
    neighborhood VARCHAR(50) NOT NULL,
    city VARCHAR(50) NOT NULL,
    venue VARCHAR(100),
    median_income INT,
    youth_pop INT,
    capacity INT
);

CREATE TABLE participants (
    participant_id INT PRIMARY KEY,
    enroll_year INT NOT NULL,
    location_id INT NOT NULL,
    age_group VARCHAR(10) NOT NULL,
    attend_rate DECIMAL(5,2),
    completed VARCHAR(3),
    outcome VARCHAR(20),
    cost_per_part INT,
    FOREIGN KEY (location_id) REFERENCES locations(location_id)
);

CREATE TABLE participants_demographics (
    participant_id INT PRIMARY KEY,
    fileAs NVARCHAR(102),
    givenname NVARCHAR(50),
    surname NVARCHAR(50),
    birthdate DATE,
    gender VARCHAR(20),
    Address_1 NVARCHAR(70),
    city NVARCHAR(35),
    province NVARCHAR(4),
    postalCode NVARCHAR(12),
    FOREIGN KEY (participant_id) REFERENCES participants(participant_id)
);

CREATE TABLE waitlist (
    year INT NOT NULL,
    location_id INT NOT NULL,
    waitlist_cnt INT,
    est_lost_part INT,
    PRIMARY KEY (year, location_id),
    FOREIGN KEY (location_id) REFERENCES locations(location_id)
);

CREATE TABLE regional_bench (
    year INT NOT NULL,
    neighborhood VARCHAR(50) NOT NULL,
    city VARCHAR(50) NOT NULL,
    college_pct DECIMAL(4,1),
    unemp_pct DECIMAL(4,1),
    trade_pct DECIMAL(4,1),
    non_trade_pct DECIMAL(4,1),
    PRIMARY KEY (year, neighborhood, city)
);

CREATE TABLE program_outcomes (
    year INT NOT NULL,
    neighborhood VARCHAR(50) NOT NULL,
    city VARCHAR(50) NOT NULL,
    college_pct DECIMAL(4,1),
    unemp_pct DECIMAL(4,1),
    trade_pct DECIMAL(4,1),
    non_trade_pct DECIMAL(4,1),
    PRIMARY KEY (year, neighborhood, city)
);
```

---

## Power Query Connection Instructions

### Connecting to GitHub-hosted CSVs:

1. **In Excel:** Data → Get Data → From File → From Web
2. **Enter URL:** `https://raw.githubusercontent.com/BOOMALATOR/youthconnect-demo-data/main/locations.csv`
3. **Transform Data** to open Power Query Editor
4. **Repeat for each table**

### Common Power Query Transformations to Demonstrate:

**For messy data:**
- Remove blank rows: Home → Remove Rows → Remove Blank Rows
- Trim whitespace: Transform → Format → Trim
- Change data types: Transform → Data Type
- Rename columns: Double-click header
- Remove columns: Right-click → Remove

---

## Sample Analysis Questions

### For Excel Demo:
- "Which location has the highest average attendance rate?"
- "What's the trend in cost per participant over time?"
- "What percentage of participants complete the program?"

### For Power BI Demo:
- "How does our college enrollment compare to regional benchmarks by neighborhood?"
- "Which locations have the highest waitlist relative to capacity?"
- "What's the correlation between attendance rate and outcomes?"

### For AI Analysis:
- "Analyze this data and recommend whether we should expand the program"
- "Which neighborhoods show the greatest impact from our program?"
- "What's the ROI of investing $220k to serve 100 more participants?"
- "Create visualizations showing our impact compared to regional averages"

---

## File Checklist for GitHub Repo

```
youthconnect-demo-data/
├── README.md (this document)
├── DATA_DICTIONARY.md
├── locations.csv
├── participants.csv
├── participants_demographics.csv
├── participants_messy.csv
├── waitlist.csv
├── regional_bench.csv
├── program_outcomes.csv
├── create_tables.sql
└── insert_data.sql
```

---

## Usage Notes

**For Presenters:**
- Start with participants_messy.csv to show Power Query value
- Use locations.csv as your first "clean" import
- Join participants to locations to show relationship concepts
- Compare program_outcomes to regional_bench for impact story
- Reference waitlist for urgency/unmet demand

**Data Integrity:**
- All percentages sum logically (college + trade + employed + other + unemployed ≈ 100%)
- Participant counts align with capacity constraints
- Cost trends are realistic (economies of scale)
- Geographic data matches real Saskatchewan neighborhoods

**Customization:**
- Feel free to adjust neighborhood names for your audience
- Can add/remove years as needed
- Can scale participant numbers up/down
- All calculations will remain proportional

---

## Contact & Attribution

**Dataset created for:** EhTech Connect 2025  
**Session:** "From Data to Decisions: Visual Storytelling with Your Numbers"  
**GitHub:** github.com/BOOMALATOR/youthconnect-demo-data  
**License:** CC0 (Public Domain) - use freely

**Questions?** Open an issue on GitHub or contact through EhOS IT Solutions.

---

*This is synthetic data created for educational purposes. Any resemblance to real programs is coincidental.*