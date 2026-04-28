# DBLP Dynamic Dataset

A temporal dataset extracted from [DBLP](https://dblp.org/), organized by year to support research on **dynamic community detection** and **temporal network analysis** in academic collaboration networks.

## Overview

This repository provides yearly snapshots of author–venue associations from DBLP, covering the period **2000–2019**. Each snapshot can be used on its own or combined across years to study how research communities and collaborations evolve over time.

## Data Format

Each `YYYY.txt` file contains one record per line, formatted as a Python-style list:

```
['Author Name', 'Venue', 'Year']
```

**Example entries from `2000.txt`:**

```
['Christian Lengauer', 'Acta Inf.', '2000']
['Arto Salomaa', 'Acta Inf.', '2000']
['Geoffrey C. Fox', 'Future Generation Comp. Syst.', '2000']
```

Fields:

- **Author Name** — author identifier as recorded in DBLP. Numeric suffixes such as `Karsten Schmidt 0004` disambiguate authors with identical names.
- **Venue** — abbreviated journal or conference name.
- **Year** — publication year (matches the filename).

## Repository Structure

```
DBLP_Dataset_dynamic/
├── 2000.txt
├── 2001.txt
├── ...
├── 2019.txt
└── README.md
```

## Usage

### Load a single year (Python)

```python
import ast

with open('2000.txt', 'r', encoding='utf-8') as f:
    records = [ast.literal_eval(line) for line in f if line.strip()]

print(f"Loaded {len(records)} records")
```

### Build an author–venue bipartite graph

```python
import ast
import networkx as nx

G = nx.Graph()
year = 2000

with open(f'{year}.txt', 'r', encoding='utf-8') as f:
    for line in f:
        if not line.strip():
            continue
        author, venue, _ = ast.literal_eval(line)
        G.add_edge(author, venue)
```

### Build a temporal sequence of graphs

```python
graphs = {}
for year in range(2000, 2020):
    G = nx.Graph()
    with open(f'{year}.txt', 'r', encoding='utf-8') as f:
        for line in f:
            if not line.strip():
                continue
            author, venue, _ = ast.literal_eval(line)
            G.add_edge(author, venue)
    graphs[year] = G
```

## Known Issues

Some entries appear truncated or contain only name fragments (for example `'gnier'`, `'meth'`, `'tos'`). These are encoding artifacts from the original DBLP XML, where non-ASCII characters (é, ä, ö, ü, etc.) were not decoded correctly during extraction. If this matters for your analysis, consider filtering out very short author strings or re-extracting from the DBLP source with proper UTF-8 handling.

## Source

Data was extracted from the public [DBLP XML dump](https://dblp.org/xml/). DBLP is maintained by Schloss Dagstuhl and is released under the [ODC-BY 1.0](https://opendatacommons.org/licenses/by/1-0/) license.

## Citation

If you use this dataset, please cite this repository along with the original DBLP source:

```bibtex
@misc{dblp_dynamic,
  author = {Mohammadi, Samaneh},
  title  = {DBLP Dynamic Dataset},
  year   = {2018},
  url    = {https://github.com/SamaneMohammadi/DBLP_dataset_dynamic}
}

@misc{dblp,
  title = {DBLP Computer Science Bibliography},
  url   = {https://dblp.org/}
}
```

## License

The contents of this repository are released under the MIT License. The underlying DBLP data is subject to its own [ODC-BY 1.0](https://opendatacommons.org/licenses/by/1-0/) license.
