Netflix Content Analysis Dashboard (Power BI)
Project overview

This project is an interactive Power BI dashboard built on a cleaned Netflix titles dataset (8,807 records, 11 columns, titles added from 2008 to September 2021). It shows what kind of content Netflix offers, where it comes from, and how the catalogue grew over time. The raw file had several data-quality problems that caused 11 load errors in Power BI, so most of the work went into diagnosing and fixing them before any visuals were built.

Tools used
Power BI Desktop
Power Query Editor (data cleaning and transformation)
Data modelling (relationships between tables)
Python/pandas, used only to inspect the CSV and find the root causes of the errors
Dataset

The columns were show_id, type, title, director, cast, country, Date Added, release_year, rating, duration and listed_in (genres).

Step 1: Diagnosing the 11 errors

Power BI created an error query listing 11 failed rows. They had two causes:

10 rows had the text “Unknown” in the date column. Power BI tried to convert the column to a date and failed on them.
1 row was split in two. The title The Memphis Belle: A Story of a Flying Fortress contains a line break. The CSV connector was set to QuoteStyle.None, which ignored quotation marks and broke the record across two rows, shifting every column.
Step 2: Data cleaning in Power Query
Fixed the broken row by changing the source setting from QuoteStyle.None to QuoteStyle.Csv. This confirmed all 8,807 show_id values were unique.
Renamed Date Added. to Date Added (it had a stray period).
Converted Date Added to a date type and replaced the 10 error values with nulls.
Repaired 3 misplaced values. Three rows had durations (74 min, 84 min, 66 min) in the rating column. I used a conditional column to move them to duration and set their rating to “Not Rated”.
Split duration into Duration Value (whole number) and Duration Unit (min / Season / Seasons) so durations can be analysed numerically.
Standardised missing values in director, cast and country. The file used unknown, UNKNOWN, Unknown and blank cells for the same thing, and I made them all “Unknown”.
Added a Year Added column extracted from Date Added, for trend analysis.
Step 3: Data modelling

country and listed_in hold several comma-separated values in one cell (for example “United States, Ghana, Germany”), so counting them directly would give wrong results. I created two supporting tables:

Country: one row per title–country pair
Genre: one row per title–genre pair

Both link to the main table through show_id with a one-to-many relationship, which lets the filters flow through the whole report.

Step 4: Visualisations
Card: total number of titles
Donut chart: Movies vs TV Shows (about 70% movies, 30% TV shows)
Line chart: titles added per year. Additions grow sharply from about 2015 and peak around 2019–2020. The 2021 drop is because the data stops in September 2021.
Bar chart: top 10 countries by number of titles. The United States leads clearly, followed by India and the United Kingdom.
Bar chart: top 10 genres
Slicer: filters the whole dashboard by Movie or TV Show

The dashboard also has a title and a colour theme applied for a consistent look.

Key insights
Movies make up roughly 70% of the catalogue.
Content additions rose steeply in the second half of the 2010s.
A small number of countries produce most of the titles, with the United States well ahead.
A noticeable share of titles have no country listed, which is a limitation worth mentioning.
