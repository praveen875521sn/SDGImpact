# ECP Impact Dashboard

Flask-based SDG 4 & 8 impact dashboard.
Reads directly from **3 raw Excel files** — no pre-processing needed.

## Data Files Required
Place these 3 files in the `data/` folder:

```
data/
├── ECP.xlsx               ← Raw ECP candidate data
├── Attendance_data.xlsx   ← Raw attendance session data
└── FA.xlsx                ← Raw formative assessment data
```

## Setup & Run Locally (PyCharm)

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Place your 3 Excel files in data/ folder

# 3. Run
python app.py
```

Open: **http://localhost:5000**

The app loads and processes all data at startup (~5–10 seconds).
After that, all API calls are instant (served from RAM).

## What it does at startup
1. Loads `ECP.xlsx` → filters to allowed statuses → 3,630 candidates
2. Loads `Attendance_data.xlsx` → aggregates per-candidate (121,867 rows → 3,389 summaries)
3. Loads `FA.xlsx` → aggregates per-candidate (91,145 rows → 2,841 summaries)
4. Merges all three by `Batch ID + Candidate ID`
5. Parses `Education Details` and `Family Details` text blocks
6. Normalises college names (groups spelling variants)
7. Builds filter lookup maps for fast cascading

## Dashboard Features

### Sidebar (always visible)
- **Multi-select filters**: Project, Centre, Batch Code, Status
- **Search**: by candidate name
- **Cascading**: selecting Project narrows Centre & Batch options
- **Live count** shows filtered candidate total

### KPI Strip (always visible)
8 live KPIs: Total, Assessed %, Certified %, Placed %, Avg Attendance, Avg FA, Female %, Centres

### Tab Pages
| Tab | Content |
|---|---|
| Overview | Pipeline, 6 charts (status, att bands, FA, monthly trend, education, state) |
| SDG 4 | 4 KPI cards, attendance/FA charts, session photos |
| SDG 8 | 4 KPI cards, placement charts, top companies, job titles |
| Centres | Searchable/filterable centre performance table |
| Candidates | Paginated card view (30/page, server-side) |
| Insights | College distribution, relationship & occupation analytics |
| Data | Attendance & FA summary tables with search |

## Deploy to Render.com (Free Hosting)
1. Create GitHub repo → push this folder (without `data/`)
2. Go to render.com → New Web Service → connect repo
3. Use **Render Disk** to upload the 3 Excel files to `/opt/render/project/src/data/`
4. Deploy

## Project Structure
```
├── app.py                  # Flask backend (~400 lines)
├── templates/
│   └── index.html          # Dashboard frontend
├── data/                   # Your Excel files (not committed to git)
│   ├── ECP.xlsx
│   ├── Attendance_data.xlsx
│   └── FA.xlsx
├── requirements.txt
├── render.yaml
└── .gitignore
```
