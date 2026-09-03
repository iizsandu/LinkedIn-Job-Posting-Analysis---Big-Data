# LinkedIn Job Posting Analysis - Big Data

## 📋 Overview

This project performs an in-depth analysis of **over 1.3 million LinkedIn job postings** using MongoDB and Python. It combines data processing, storage, and analytical techniques to extract meaningful insights about the job market, required skills, geographical distribution, and employment trends.

**Technologies Used:**
- Python (Pandas, NumPy)
- MongoDB (NoSQL Database)
- Data Visualization (Matplotlib, Seaborn)
- NLP & Text Processing (Gensim, WordCloud, Folium)
- Geospatial Analysis (GeoPy)

---

## 📊 Project Structure

```
├── job_listings.ipynb          # Main analysis notebook
├── README.md                   # This file
└── data/
    └── LinkedIn_Jobs/
        ├── linkedin_job_postings.csv    # Job posting metadata (1.3M+ records)
        ├── job_summary.csv              # Job descriptions/summaries
        └── job_skills.csv               # Required skills per job
```

---

## 🎯 Project Objectives

1. **Data Integration**: Merge three large datasets (job postings, summaries, skills) based on common job links
2. **Data Storage**: Store cleaned, merged data in MongoDB for efficient querying
3. **Exploratory Analysis**: Discover patterns in job market trends, locations, and requirements
4. **Visualization**: Create meaningful visualizations for market insights
5. **Skill Analysis**: Identify most in-demand skills across industries and regions

---

## 📈 Dataset Summary

| Dataset | Records | Key Columns |
|---------|---------|------------|
| **Job Postings** | 1,348,454 | job_link, job_title, company, job_location, job_level, job_type, search_position, search_city, search_country |
| **Job Summaries** | 1,297,332 | job_link, job_summary (description) |
| **Job Skills** | 1,296,381 | job_link, job_skills (comma-separated) |
| **Final Merged** | 1,294,296 | All above fields combined |

### Data Features

- **Job Details**: Title, Company, Location, Level, Type (On-site/Remote/Hybrid)
- **Geographic Info**: Search city, search country, actual job location
- **Temporal Data**: First seen date, last processed time
- **Content**: Full job descriptions and required skills
- **Processing Status**: Flags for summary extraction and NER (Named Entity Recognition)

---

## 🔧 Workflow & Analysis Steps

### 1. **Data Loading & Exploration**
```python
# Load three separate CSV files
job_posting = pd.read_csv("linkedin_job_postings.csv")
job_summary = pd.read_csv("job_summary.csv")
job_skills = pd.read_csv("job_skills.csv")
```

**Initial Statistics:**
- Job Postings: 1,348,454 records × 14 columns
- Job Summaries: 1,297,332 records × 2 columns
- Job Skills: 1,296,381 records × 2 columns

### 2. **Data Integration**
- Find common job links across all three datasets
- Perform inner joins to create unified dataset
- **Result**: 1,296,381 complete records with all information
- **Key Insight**: 96% data coverage across all three sources

### 3. **Data Cleaning**
- Remove null values in critical columns (company, job_location, job_skills)
- Final clean dataset: 1,294,296 records
- Minimal data loss (0.15%) indicates good data quality

### 4. **MongoDB Storage**
- Connect to local MongoDB instance (localhost:27017)
- Create "BigData" database with "JobListings" collection
- Insert all records as BSON documents
- Create indexes on frequently queried fields:
  - `job_title` (fast job title searches)
  - `job_location` (geographic filtering)
  - `first_seen` (temporal queries)

### 5. **Analysis & Visualization**
The notebook includes analysis for:
- **Geographic Distribution**: Jobs by location and country
- **Skill Analysis**: Most demanded skills, skill frequency
- **Job Market Trends**: Job levels, employment types, salary expectations
- **Text Analysis**: Word clouds, topic modeling with LDA
- **Geospatial Visualization**: Interactive maps with Folium

---

## 📚 Libraries & Dependencies

### Data Processing
- `pandas` - Data manipulation and analysis
- `numpy` - Numerical computing

### Database
- `pymongo` - MongoDB driver for Python
- `bson` - BSON serialization

### Visualization
- `matplotlib` - Static plotting
- `seaborn` - Statistical data visualization
- `folium` - Interactive geospatial maps
- `wordcloud` - Word frequency visualization

### NLP & Text Analysis
- `gensim` - Topic modeling (LDA - Latent Dirichlet Allocation)
- `collections.Counter` - Frequency counting

### Geospatial
- `geopy` - Geographic coordinate manipulation
- Nominatim geocoder for location services

### Utilities
- `re` - Regular expressions for text processing
- `json` - JSON data handling
- `os` - Operating system interfaces
- `time` - Timing functions
- `random` - Random sampling

---

## 🚀 Getting Started

### Prerequisites
```bash
# Install required packages
pip install pandas numpy pymongo matplotlib seaborn folium gensim wordcloud geopy
```

### Running the Analysis

1. **Ensure MongoDB is running**:
   ```bash
   mongod --dbpath /path/to/data/directory
   ```

2. **Open the Jupyter notebook**:
   ```bash
   jupyter notebook job_listings.ipynb
   ```

3. **Execute cells in order**:
   - Load data from CSV files
   - Explore and merge datasets
   - Connect to MongoDB
   - Insert and index data
   - Perform analysis and generate visualizations

---

## 📊 Key Findings & Insights

### Data Coverage
- **95%+ coverage** when merging three datasets
- **Most complete records**: Job titles, locations, company names
- **Missing data**: Minimal, mostly in skills field (0.16%)

### Job Market Distribution
- Jobs span **multiple countries** (primarily USA-based in dataset)
- **Mid-senior level** roles are dominant
- Mix of **on-site and remote** opportunities
- Diverse **company sizes** represented

### Skill Insights
The analysis extracts and analyzes:
- Technical skills (programming languages, frameworks)
- Domain-specific skills (industry expertise)
- Soft skills (communication, leadership)
- Certification requirements

### Geospatial Analysis
- Interactive maps show job concentration areas
- Clustering analysis identifies job market hotspots
- Location-based skill requirements

---

## 🗂️ Data Schema (MongoDB)

Each job record contains:
```json
{
  "_id": ObjectId("..."),
  "job_link": "https://www.linkedin.com/jobs/view/...",
  "job_title": "Senior Software Engineer",
  "company": "Tech Corp",
  "job_location": "San Francisco, CA",
  "job_level": "Mid senior",
  "job_type": "Onsite",
  "search_city": "San Francisco",
  "search_country": "United States",
  "search_position": "Software Engineer",
  "first_seen": "2024-01-15",
  "job_summary": "Full job description...",
  "job_skills": "Python, Java, SQL, AWS, Docker, ...",
  "last_processed_time": "2024-01-21T...",
  "got_summary": "t",
  "got_ner": "t",
  "is_being_worked": "f"
}
```

---

## 💡 Use Cases

1. **Job Seeker Insights**: Identify in-demand skills and market trends
2. **Career Planning**: Understand progression paths and required qualifications
3. **Recruitment Analysis**: Benchmark hiring trends across industries
4. **Skill Gap Analysis**: Find training opportunities in high-demand areas
5. **Geographic Analysis**: Discover job markets in different regions
6. **Market Research**: Monitor employment trends over time

---

## 📈 Analysis Outputs

### Visualizations Generated
- **Bar charts**: Top skills, job titles, companies
- **Word clouds**: Skill frequency and prominence
- **Geographic maps**: Job distribution by location
- **Topic models**: Job market segments via LDA
- **Statistical summaries**: Comprehensive insights

---

## 🔍 Data Quality Notes

- **Data Volume**: 1.3M+ records ensure statistical significance
- **Completeness**: 95%+ data merging success rate
- **Consistency**: Common field (job_link) ensures data integrity
- **Recency**: Data captured through January 2024

---

## 🔐 MongoDB Indexing Performance

Created indexes on:
- **job_title**: Fast title-based queries across 1M+ unique titles
- **job_location**: Geographic filtering with 50K+ unique locations
- **first_seen**: Temporal queries and time-series analysis

These indexes significantly improve query performance on this large dataset.

---

## 📝 Future Enhancements

- [ ] Salary data extraction and analysis
- [ ] Skill trend analysis over time
- [ ] Machine learning predictions (salary, job fit)
- [ ] Network analysis of company hiring patterns
- [ ] Advanced NLP for job description clustering
- [ ] Real-time data updates
- [ ] Interactive dashboard (Dash/Streamlit)

---

## 💬 Notes

- This analysis is for educational and research purposes
- All data is anonymized and aggregated for insights
- Results are representative of the job market snapshot captured
- MongoDB ensures scalable storage for big data processing

---

## 👤 Author

**iizsandu**

---

**Last Updated**: January 2024  
**Dataset Size**: 1.3 Million+ Job Records  
**Status**: ✅ Complete Analysis
