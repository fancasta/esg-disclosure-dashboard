# ESG Disclosure Dashboard

**Overview** · [Architecture](ARCHITECTURE.md) · [Live explorer](https://fancasta.github.io/climate-disclosure-explorer/) · [Research pipeline](https://github.com/fancasta/climate-disclosure-explorer)

A banker preparing for a financing discussion needs to check which emissions a company’s pledge covers and what progress it can show. This NUS capstone for **NatWest Markets** (August–December 2022) linked nine climate indicators to evidence in company reports. The public explorer extends that work to 27 reports and 3,035 searchable PDF pages across environmental, social and governance topics.

**Try it with Shell.** Its 2021 report set a 2030 emissions target and later listed asset sales among the reasons emissions fell. Open the two pages and ask how much of the decline came from improvements at sites Shell kept operating. [Read the target](https://fancasta.github.io/climate-disclosure-explorer/?publisher=Shell&page=92) · [Check the reported progress](https://fancasta.github.io/climate-disclosure-explorer/?publisher=Shell&page=95)

[![Homepage with a worked financing discussion using Shell’s 2021 report](docs/esg-home.png)](https://fancasta.github.io/climate-disclosure-explorer/)

| Guided search | Matching pages | Reports |
| --- | ---: | ---: |
| [Water stewardship](https://fancasta.github.io/climate-disclosure-explorer/?topic=Water+stewardship) | 126 | 24 |
| [Collective bargaining](https://fancasta.github.io/climate-disclosure-explorer/?topic=Collective+bargaining) | 55 | 16 |
| [Board independence](https://fancasta.github.io/climate-disclosure-explorer/?topic=Board+independence) | 24 | 9 |

## Follow a finding to its source

1. Open a question from Home, or search the directory by keyword, topic or company.
2. Compare the matching companies. Each count is the number of matching PDF pages in that company’s report. Use the sector filter to narrow the list.
3. Select a company to read its passages, then open a cited page to inspect the report in context.

Try [methane leakage](https://fancasta.github.io/climate-disclosure-explorer/?q=methane+leakage) as a keyword search or [renewable electricity in the diversified sector](https://fancasta.github.io/climate-disclosure-explorer/?topic=Renewable+electricity&sector=Diversified). Archived reports open in a page viewer with selectable text. The three later technology publications also appear in the directory.

[![Company results for renewable electricity in the diversified sector](docs/esg-results.png)](https://fancasta.github.io/climate-disclosure-explorer/?topic=Renewable+electricity&sector=Diversified)

## Local reporting workspace

The original React workspace groups reports by industry and year and lets a reader inspect the sentences behind nine indicators. In the fictional 2024 manufacturing sample below, five of six reports disclose a net-zero goal and three describe a transition plan. The bars count disclosures in the selected group. [Sample data](examples/mock-reports.json) · [Aggregation logic](frontend/src/components/disclosures.js) · [Architecture and data flow](ARCHITECTURE.md#how-the-capstone-compares-companies)

![Screenshot of the running React summary with six fictional manufacturing reports, five net-zero goals and three transition plans](docs/disclosure-summary.png)

## Run locally

With Docker running, execute `docker compose up --build` from the repository root. Open `http://localhost:3000/workspace`, select a company and inspect its reports, or choose a year and industry in the summary chart. The API uses port `6868` and the local Adminer database viewer uses port `8000`. Run `docker compose down` to stop the services.

## Build the public demo

In `frontend`, run `npm ci`, set `REACT_APP_PUBLIC_DEMO=true`, `PUBLIC_URL=/climate-disclosure-explorer` and `GENERATE_SOURCEMAP=false`, then run `npm run build`. The build includes the page index and a PDF.js reader. The research repository serves these files from its `gh-pages` branch. The local full-stack app also exposes the public search at `/explore`.

The [source manifest](search/sources.json) pins PDF editions with SHA-256. To rebuild the index, install PyMuPDF, clone the [research repository](https://github.com/fancasta/climate-disclosure-explorer), and download its three [additional publisher PDFs](https://github.com/fancasta/climate-disclosure-explorer/blob/main/examples/public-reports.json) into a separate folder. Run `python search/build_index.py PATH_TO_RESEARCH PATH_TO_PUBLISHER_PDFS` from this repository. The index retains physical page numbers and excludes seven pages without indexable terms.

[Topic phrases](search/topics.json) · [Index builder](search/build_index.py) · [BM25 ranking and filters](frontend/src/corpusSearch.js)
