# SQL-learning-journal---day-39

# Day 39 – Boston AirBnB Data Views

Today I worked on creating multiple SQL views to analyze Boston’s AirBnB data from the `bnb.db` database. This exercise helped me practice joins, filtering, aggregation, and view creation — all key skills for data analysis with SQL.

### Views created:

- **available**: Lists all available dates for all listings.
- **frequently_reviewed**: Shows the top 100 most frequently reviewed listings, ordered by review count and then alphabetically by property type and host name.
- **june_vacancies**: Counts the number of days listings were vacant (available) during June 2023.
- **no_descriptions**: Lists all listing details except descriptions to avoid cluttered text output.
- **one_bedrooms**: Filters listings that have exactly one bedroom.

### Sample commands I used:
```bash
sqlite3 bnb.db < available.sql
sqlite3 bnb.db < frequently_reviewed.sql
sqlite3 bnb.db < june_vacancies.sql
sqlite3 bnb.db < no_descriptions.sql
sqlite3 bnb.db < one_bedrooms.sql
I’m really enjoying learning how powerful and elegant SQL is for querying real-world datasets. It’s exciting to see how simple commands can uncover rich insights!

quarries used today:

CREATE VIEW available AS
SELECT l.id, property_type, host_name, date
FROM listings l
JOIN availabilities a ON l.id = a.listing_id
WHERE available = 'TRUE';

CREATE VIEW frequently_reviewed AS
SELECT l.id, l.property_type, l.host_name, COUNT(r.id) AS reviews
FROM listings l
JOIN reviews r ON l.id = r.listing_id
GROUP BY l.id, l.property_type, l.host_name
ORDER BY reviews DESC, property_type ASC, host_name ASC
LIMIT 100;

CREATE VIEW june_vacancies AS
SELECT l.id, l.property_type, l.host_name, COUNT(a.date) AS days_vacant
FROM listings l
JOIN availabilities a ON l.id = a.listing_id
AND a.available = 'TRUE'
AND a.date BETWEEN '2023-06-01' AND '2023-06-30'
GROUP BY l.id, l.property_type, l.host_name
limit 10;


CREATE VIEW no_descriptions AS
select
    "id",
    "property_type",
    "host_name",
    "accommodates",
    "bedrooms"
FROM listings;


CREATE VIEW one_bedrooms AS
SELECT id, property_type, host_name, accommodates
FROM listings
WHERE bedrooms = 1;
