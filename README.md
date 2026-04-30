[README_StudentPlatform.md](https://github.com/user-attachments/files/27255579/README_StudentPlatform.md)
<div align="center">

<img src="https://img.shields.io/badge/Amazon%20QuickSight-8C4FFF?style=for-the-badge&logo=amazonaws&logoColor=white"/>
<img src="https://img.shields.io/badge/Amazon%20S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white"/>
<img src="https://img.shields.io/badge/Amazon%20Athena-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white"/>
<img src="https://img.shields.io/badge/AWS%20Cloud%20Analytics-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white"/>

# 📊 AWS Student Intelligence Platform

**A cloud analytics solution that transforms raw student enrollment data into actionable dashboards for academic and financial decision-making.**  
Built on Amazon Web Services using S3, Athena, and QuickSight — from raw CSV to interactive BI dashboards with natural language Q&A.

[![LinkedIn](https://img.shields.io/badge/Meshal%20Ahmed-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/meshal-ahmed-4a84242b5)
[![GitHub](https://img.shields.io/badge/2027--2003-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/2027-2003)

</div>

---

## 📌 What It Does

| Input | Process | Output |
|-------|---------|--------|
| Raw student enrollment CSV (1,000 records) | Ingested into S3 → queried via Athena → visualized in QuickSight | Interactive dashboards for college administrators |
| Natural language questions | Amazon Q Topics with Named Entities & synonyms | Auto-generated charts and written insights |
| Business scenario prompt | QuickSight Scenarios (Data to Insights) | AI-generated analysis with suggested follow-up questions |

---

## 🏗️ Architecture


<img width="1490" height="812" alt="image" src="https://github.com/user-attachments/assets/f47022d9-cd44-4647-b62a-7c6e4bf65a1a" />



### Problem Description (S3 Manifest Error)

The image shows an error encountered while connecting a data source in Amazon QuickSight using a manifest file from Amazon S3. The system was unable to parse the manifest file as valid JSON.

**Cause of the Issue:**
The manifest file contained invalid JSON formatting or an incorrect file path in S3.

**Solution:**

* Ensure the manifest file is written in valid JSON format
* Verify that the S3 file path is correct
* Check for common issues such as trailing commas or incorrect structure
* Re-upload the corrected manifest file

**Result:**
The issue was resolved successfully, and the dataset was connected to QuickSight without errors.



```
[CSV Dataset — Student Enrollment Data]
         │
         ▼
   [Amazon S3]                ← Raw data lake
   RegionalCommunityCollegeBucket/EnrollmentData/
         │
         ▼
   [Amazon Athena]            ← SQL queries over S3 data
         │
         ▼
   [Amazon QuickSight]        ← Dashboards, Q&A, Scenarios, Stories
         ├── SPICE Engine      ← In-memory dataset (7,306 rows ingested)
         ├── Amazon Q Topics   ← Natural language question interface
         ├── Scenarios         ← AI-driven business analysis
         └── Stories           ← Presentation-ready data narratives
```

---

## 🛠️ Tech Stack

| Service | Role |
|---------|------|
| **Amazon S3** | Data lake — stores the raw enrollment CSV |
| **Amazon Athena** | SQL queries directly over S3 without loading data |
| **Amazon QuickSight** | BI dashboards, charts, and interactive visuals |
| **SPICE Engine** | QuickSight's in-memory cache — 7,306 rows, weekly refresh |
| **Amazon Q Topics** | Natural language Q&A over the dataset |
| **QuickSight Scenarios** | AI-powered business scenario analysis |
| **QuickSight Stories** | Narrative reports combining visuals + written insights |

---

## 📁 Dataset

**Source:** Regional Community College Student Enrollment Data  
**Size:** 1,000 students · 7,306 rows in SPICE after refresh

### Fields

| Field | Type | Description |
|-------|------|-------------|
| `StudentId` | String | Unique student identifier |
| `StudentName` | String | Full name |
| `AcademicYear` | Integer | Enrollment year (2019–2024) |
| `Major` | String | Field of study |
| `StudentClassification` | String | Freshman / Sophomore / Junior / Senior / Graduate |
| `Age` | Integer | Student age |
| `Gender` | String | Gender |
| `NationalOrigin` | String | Country of residence on admission application |
| `EnrollmentDate` | Date | Date of enrollment |
| `GraduationDate` | Date | Graduation date (if applicable) |
| `Course` | String | Course name |
| `Professor` | String | Instructor name |
| `CostPerCourse` | Measure | Course cost ($) |
| `EvaluationScore` | Measure | Student evaluation score (65–99) |
| `Grade` | String | Letter grade |
| `Credit` | Integer | Credit hours |
| `Semester` | String | Fall / Spring |

### Calculated Field

```
Student Type = ifelse(Age < 30, 'Youth', 'Adult Continuing Education')
```

Segments the student body by age group to support flexible program planning.

---

## 📊 Dashboards & Visuals

### 1. Student Majors by Year
A grouped bar chart showing enrollment counts across majors (Economics & Finance, Communication, Biology, Art History, Accounting, Computer Science) broken down by academic year 2019–2024. Helps administrators identify growing or declining programs.

### 2. Proportion of Student Types
A donut chart showing classification breakdown across 1,000 students:

| Classification | Count | % |
|---------------|-------|---|
| Graduate | 262 | 26% |
| Junior | 229 | 23% |
| Senior | 192 | 19% |
| Freshman | 180 | 18% |
| Sophomore | 137 | 14% |

### 3. Average Evaluation Score by Professor
Horizontal bar chart ranking 17 professors by average student evaluation score. Top performer: **Jill at 78.63**. Overall average: **75.69**.

### 4. Average Cost Per Course
Horizontal bar chart showing the most expensive courses on average:

| Course | Avg Cost |
|--------|----------|
| Big Data | $2,690 |
| Investment | $2,670 |
| Environmental Ethics | $2,620 |
| Communication | $2,620 |
| Accounting | $2,040 |

---

## 🤖 Amazon Q Topics — Natural Language Q&A

Configured a Q Topic called **"Regional Community College Student Data"** with the following setup:

**Named Entities** (field groupings for smarter Q&A):
- `Student Details` → StudentName, Semester, Course, TestScore, Grade, StudentClassification, Student Type, Major, Gender, NationalOrigin, Credit, EnrollmentDate, GraduationDate, StudentId
- `Course Details` → Course, Professor, CostPerCourse, AcademicYear, Semester, CourseId
- `Professor Evaluation` → Professor, Course, Semester, AcademicYear, StudentName, EvaluationScore

**Field synonyms configured** (sample):
- `AcademicYear` → school year, academic year
- `CostPerCourse` → price per course, cost per class, tuition per course
- `Age` → years old
- `City` → locality

**Sample Q&A Results:**

> *"Which instructors got the best average evaluations?"*  
> → Average EvaluationScore = **75.69** · Top professor: Jill at 78.63 · 17 unique professors · Total AcademicYear: 14,767,070

> *"Which courses are the most expensive on average?"*  
> → Average CostPerCourse = **$2,063.89** · 15 unique courses · Highest: Big Data at $2,692.58

> *"How many studentids by semester?"*  
> → 1,000 unique StudentIds · Fall: 925 · Spring: 845

---

## 💡 Scenarios — AI Business Analysis

Used QuickSight Scenarios (Data to Insights) to answer:

> *"How do we improve professor evaluations, while avoiding an increase in cost per course?"*

The system generated follow-up analytical threads including:
- What factors are contributing to higher evaluation scores? Is there a correlation between EvaluationScore and CostPerCourse?
- Show the trend of average EvaluationScore by AcademicYear
- Who are the top 5 professors with the highest average EvaluationScore?
- How would reducing CostPerCourse by 10% for courses with EvaluationScore below 75 impact overall costs?

---

## 📖 QuickSight Story

Created a data story titled **"Unlocking Student Success: A Data-Driven Approach to Satisfaction and Efficiency"** combining visuals with written narrative — formatted for presentation to the college's administration.

---

## 🔧 Setup Notes

### S3 Manifest File Fix
During initial setup, a manifest file error occurred when connecting S3 to QuickSight. The error was caused by an incorrect file path inside the manifest JSON. After correcting the S3 URI and re-uploading, the dataset loaded successfully.

```json
{
  "fileLocations": [
    {
      "URIs": [
        "s3://RegionalCommunityCollegeBucket/EnrollmentData/student_enrollment.csv"
      ]
    }
  ],
  "globalUploadSettings": {
    "format": "CSV",
    "delimiter": ",",
    "containsHeader": "true"
  }
}
```

### SPICE Refresh Schedule
- **Refresh type:** Full refresh
- **Occurrence:** Weekly (Sunday)
- **Start time:** 19:54 Asia/Riyadh
- **Last sync:** 7,306 rows ingested · 0 skipped

---

## 📈 Business Insights

| Insight | Action |
|---------|--------|
| Computer Science shows highest 2024 enrollment | Prioritize CS faculty hiring and lab resources |
| Big Data and Investment are most expensive courses | Review cost vs. enrollment ratio for ROI analysis |
| Jill scores highest in professor evaluations (78.63) | Use as benchmark for teaching practices |
| 26% of students are Graduate level | Consider expanding graduate program offerings |
| Fall semester has 80 more students than Spring | Plan faculty and resource allocation accordingly |

---

## ✅ Conclusion

This project demonstrates a full cloud analytics pipeline on AWS — from raw CSV ingestion in S3 through SQL querying with Athena, to interactive dashboards and natural language Q&A in QuickSight. The result is a working BI platform that helps a college make data-driven decisions around enrollment, faculty performance, and course costs.

---

<div align="center">
<sub>Built with ☁️ on AWS &nbsp;·&nbsp; Amazon S3 &nbsp;·&nbsp; Amazon Athena &nbsp;·&nbsp; Amazon QuickSight &nbsp;·&nbsp; Amazon Q</sub>
</div>
