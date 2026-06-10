# HR Analytics Dashboard (Power BI)

An end-to-end HR analytics dashboard built in **Power BI** on a **star-schema** data model. Three report pages cover workforce, attrition, and HR service-desk SLAs, using point-in-time and time-intelligence **DAX** measures, with documented metric definitions and data-quality rules.

**[Live interactive dashboard](https://app.powerbi.com/view?r=eyJrIjoiNWNkMDQ4ZWItZjdlNC00YTBlLThhYTEtMDAyN2YyMDE3YzhlIiwidCI6IjVhNGVmZTAwLTg2MjMtNGUyMS05ZDUwLTE0YzA5OWU2ZWU3OSIsImMiOjZ9)**

> Data is **synthetic** (generated for this project). No real personal data.

## Pages

**1. HR Overview:** point-in-time headcount, new hires, annualized attrition rate, average tenure, diversity (female %), and headcount YoY; a monthly trend of hires and terminations vs. active headcount; headcount by department; slicers for year, department, and country, synced across all pages.

**2. Attrition:** annualized attrition rate by department; a department-by-year attrition heat-map (matrix) with a colorblind-safe sequential palette; terminations by seniority. Rates are suppressed for groups below a minimum headcount, and the page explains its own blanks via a dynamic, measure-driven subtitle.

**3. SLA:** ticket volume, SLA compliance %, average resolution time; tickets by priority; average resolution by priority; and monthly SLA compliance against an 85% target line. The year filter on this page only offers years with ticket data, and the page explains itself when a selection carried over from another page has no tickets.

## Data model (star schema)

- **Fact:** `FactTickets`
- **Dimensions:** `Dim_Employee`, `Dim_Department`, `Dim_Location`, `Dim_Date`
- Role-playing date relationships (hire date and termination date) are kept **inactive** and activated per-measure via `USERELATIONSHIP`, so the model stays unambiguous.

## Key DAX

- **Point-in-time Active Headcount:** counts everyone hired on or before the end of the selected period who has not left yet (`MAX(Date)` boundary plus `FILTER`), so it recalculates correctly across every month and by any breakdown (department, location, gender).
- **New Hires / Terminations:** `USERELATIONSHIP` on the inactive hire and termination-date relationships.
- **Attrition Rate % (annualized):** terminations ÷ average monthly headcount, scaled to a 12-month basis (×12 / months in the selected period) so multi-year selections show a comparable average annual rate instead of a misleading cumulative one. Returns blank where average headcount < 10, because tiny denominators produce statistically unreliable percentages.
- **Dynamic, measure-driven subtitles:** DAX measures return conditional text (bound to visuals via conditional formatting with field values), so the report explains suppressed or missing data instead of showing unexplained blanks.
- **SLA Compliance %**, **Avg Resolution (Hours)**, **Headcount YoY %**, including time intelligence.

## Metric definitions & data rules

- **Active Headcount** — point-in-time: everyone hired on or before the end of the selected period whose termination date is blank or later than it.
- **Attrition Rate %** — annualized (see above). Suppression rule: not shown where average headcount < 10.
- **Current year** — partial-year figures are year-to-date, not annualized.
- **SLA Compliance %** — share of tickets resolved within target hours; ticket data starts Jan 2023, and the SLA page's year filter only offers years with ticket data.
- Filters are synced across pages; any visual that deliberately ignores a filter says so in its title.

## Skills demonstrated

Power BI Desktop, DAX (CALCULATE, filter context, time intelligence, USERELATIONSHIP, point-in-time measures), Power Query (ETL and type handling), star-schema modeling, conditional formatting, role-playing dimensions, publishing to the Power BI Service, dynamic measure-driven titles/subtitles (conditional formatting with field values), synced slicers, metric governance (suppression thresholds, documented definitions), and accessibility-aware design.

## Screenshots

| Overview | Attrition | SLA |
|---|---|---|
| ![Overview](screenshots/overview.png) | ![Attrition](screenshots/attrition.png) | ![SLA](screenshots/sla.png) |

## Repository contents

- `HR_Dashboard.pbix` : the Power BI report (open in Power BI Desktop).
- `screenshots/` : page screenshots.
- `hr_data/` : synthetic source CSVs.

---

Built by Miloš Šarenac, using AI-assisted workflows with all metrics manually defined, validated, and documented. [LinkedIn](https://www.linkedin.com/in/milo%C5%A1-%C5%A1arenac-033586141/)
