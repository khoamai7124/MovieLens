# MovieLens Analysis: Ratings, Tags and Genres

Data wrangling and exploratory analysis of the MovieLens dataset in Python. Six raw tables are cleaned and merged into one analysis-ready dataset, which is then used to answer five questions about ratings, tagging, release year and genre.

## Questions

1. **Genre:** how do average ratings differ across genres?
2. **Tags and engagement:** do tagged movies behave differently from untagged movies?
3. **Release year:** how do ratings, popularity and tagging change with release year and decade?
4. **Genre combinations:** do multi-genre movies get a different audience response than single-genre movies?
5. **Taste over time:** which genres rose or fell in average rating between 2000 and 2019 (by the year the rating was submitted)?

## Data

Source: MovieLens ([GroupLens](https://grouplens.org/datasets/movielens/)). The raw CSV files are not included in this repository; The analysis uses a curated subset of the MovieLens 25M dataset, prepared and provided in the `data/` folder. This subset is smaller than the full public MovieLens dataset (25 million ratings, 62,000 movies) so that it can be comfortably processed in Google Colab without specialized hardware. 

| File | Rows | What one row is |
|---|---|---|
| `ratings.csv` | 192,820 | one user's rating of one movie |
| `movies.csv` | 8,305 | one movie (title, genres) |
| `links.csv` | 8,305 | one movie's IMDb/TMDB IDs |
| `tags.csv` | 470,006 | one tag applied by a user to a movie |
| `genome_score.csv` | 81,216 | one movie-tag relevance score |
| `genome_tags.csv` | 1,128 | one genome tag name |

## Approach

**Cleaning** (the rules are explained in the report):
- Ratings: 5,141 null ratings dropped, 7,712 negative ratings flipped to positive (sign errors), 2,572 ratings above 5 dropped. 185,107 ratings remain.
- Genome relevance: negatives flipped, values above 1 and nulls dropped.
- Tags: whitespace stripped, lower-cased, empty tags dropped.
- Movies with no tags or no genome data are kept (left as NaN, counted as 0 tags in the analysis).

**Merging:** `ratings` is the fact table. `tags` and `genome_score` have many rows per movie, so they are summarised to one row per `movieId` before being joined. A direct join on `movieId` would repeat each rating once per tag row and produce tens of millions of rows. The keys are `movieId` for movie-level joins and `tagId` for `genome_score` + `genome_tags`.

**Result:** 185,107 rating rows, 8,194 movies, 78,411 users, 17 columns.

## Key findings

- **Genre:** Film-Noir has the highest average rating (3.98) and Horror the lowest (3.33).
- **Tags:** tagged movies average 3.19 and 23.56 ratings; untagged movies average 2.78 and 1.27 ratings. Untagged movies are mostly rated by a single person, so the quality gap should be read with care.
- **Release year:** average movie rating falls from 3.58 (1930s) to 3.07 (2000s), while the number of movies and tags is concentrated in the 1990s and early 2000s.
- **Genre combinations:** multi-genre movies get about 30 ratings each vs 10 for single-genre movies, but their average rating is almost the same (3.19 vs 3.14).
- **Taste over time:** from 2000 to 2019, Horror (+0.22), Fantasy (+0.18) and Sci-Fi (+0.18) rose; Musical (−0.18) fell, but not steadily.

## Repository structure

```
.
├── README.md
├── MovieLens_Analysis.ipynb     # cleaning, merging and analysis
├── report/
│   └── MovieLens_Report.docx    # written report
├── data/                        # raw CSVs go here
├── figures/                     # exported charts
└── requirements.txt
```

## How to run

1. Install the requirements: `pip install -r requirements.txt` (pandas, numpy, matplotlib, seaborn, jupyter).
2. Put the six CSV files in `data/` (the notebook was written for Google Colab, so if you run it locally, change the file paths at the top of the notebook).
3. Open `MovieLens_Analysis.ipynb` and choose **Restart and run all** so that every output is regenerated from the current code.

## Limitations

- Release years in this dataset end in 2004, so release-decade trends say nothing about recent films.
- `tag_count` counts every tag record, not distinct tags.
- The comparisons are descriptive; no significance tests or causal claims are made.
- Some genre-year averages in Q5 rest on only a few hundred ratings.

## Author

Anh Khoa Mai · La Trobe University Student · maverik7124@outlook.com

## License and data use

MovieLens dataset is a public movie recommendation dataset containing ratings, tags, movie metadata, and lookup tables. The dataset is organized across multiple linked files, making it suitable for data integration, cleaning, transformation, and exploratory analysis. 
