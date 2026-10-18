# Malicious URL Explorer

## Authors

- **Author #1: Thel Nu Zaw** — [@ThelNuZaw](https://github.com/ThelNuZaw) — data collection, storage, and Fetch page.
- **Author #2: Kiana Bakrani** — [@kiana-bakrani](https://github.com/kiana-bakrani) — data analysis, visualization, and Results page.

## Project Description

Malicious URL Explorer is a Python application that uses URLhaus data to explore reported malware distribution URLs by threat category, tag, hosting country, and date.
Users will collect recent records through a Flask web interface and filter the dataset by date and threat category or tag.
The application will store defanged URL records locally as CSV files, with cached hosting-country lookups to reduce repeated requests.
It will summarize URL counts by country and category, calculate daily additions and offline percentages, and estimate observed online durations when repeated status observations are available.
An interactive world map and a time-series chart will help users compare geographic patterns and changes over time without opening the reported URLs.

## Project Outline / Plan

### Interface Plan

We plan to build a Python Flask application with two pages, one implemented by each author.

| Page | Owner | Planned interactive controls |
| --- | --- | --- |
| Fetch | Thel Nu Zaw (Author #1) | Start-date input, end-date input, threat/tag filter, Fetch button, preview-table pagination control, Results navigation button |
| Results | Kiana Bakrani (Author #2) | Country dropdown, map/time-series toggle, threat/tag checkboxes, status dropdown, Save Plot button, Fetch navigation button |

This provides at least twelve interactive controls across two pages. The Fetch page will also show a progress/status label and a preview table. The Results page will show the selected visualization and summary statistics, and display a clear message when filters return no records. The shared interface outline is preliminary; each author will refine their own page during implementation.

### Data Collection and Storage Plan (written by Author #1)

**Thel Nu Zaw will write and commit this section.** It remains reserved for his own data-source, collection, CSV-storage, and country-cache plan.

### Data Analysis and Visualization Plan (written by Author #2)

**Author: Kiana Bakrani**

I will use pandas to clean and analyze the CSV records supplied by Author #1, parse dates in UTC, and remove duplicate URL records using the URLhaus record ID where available. I will group records by hosting country and threat category, compare malware tags where the threat field has little variation, and count newly reported URLs per day using their date-added values. Daily counts describe additions to URLhaus rather than the dates attacks first occurred. Missing hosting-country values will remain in an Unknown group and will be reported separately from countries plotted on the map.

I will calculate the percentage of records marked offline within the selected dataset and filters, showing the record counts alongside each percentage. Unknown or missing status values will be reported separately and excluded from the online/offline percentage denominator. If repeated timestamped status observations are available, I will estimate observed online durations from the first online observation to the first subsequent offline observation; URLs still online will be marked ongoing. A single snapshot will support status percentages, but will not be treated as evidence of a URL's full online lifetime.

I will create a Plotly world choropleth map showing URL counts by hosting country and a time-series chart showing newly reported URLs per day, split by threat category or selected malware tags. Each plot will include a descriptive title, date range, legend or color scale, and labeled axes where applicable. Hosting country represents infrastructure location, not the location or nationality of an attacker. Since a record can have several tags, tag-group totals can overlap and will be labeled accordingly.

I will implement the Results page with country, threat/tag, and status filters, a map/time-series toggle, and a Save Plot button that exports the current visualization as HTML. I will also save summary CSV files by country, threat category, and date under `data/summaries/`, keeping downloaded records and generated summaries out of Git.

## Data Source and Access

The planned source is the [URLhaus Community API](https://urlhaus.abuse.ch/api/). Its recent export covers URLs added within the past 30 days, so available date filters will reflect the downloaded coverage. Access requires a free abuse.ch Auth-Key, which will be kept in local configuration and excluded from Git. Downloads will respect the documented minimum five-minute refresh interval.

URLhaus focuses on malware distribution URLs; the threat field may offer limited variety, so malware tags will provide additional comparison categories. The application will process metadata and display URLs as defanged text without fetching their contents.

## License

Project code uses the [BSD 3-Clause License](LICENSE). URLhaus data remains subject to its provider's terms and attribution requirements.
