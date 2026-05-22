# データベース実習　期末試験

## 1

```sql
SELECT Population FROM city WHERE name = 'Tokyo';

EXPLAIN ANALYZE SELECT Population FROM city WHERE name = 'Tokyo';

DROP INDEX IF EXISTS idx_name ON city;
CREATE INDEX idx_name ON city(name);
```

## 2

```sql
SELECT DISTINCT CountryCode, language FROM countrylanguage WHERE IsOfficial = 'T' AND Percentage >= 98.0;

EXPLAIN ANALYZE SELECT DISTINCT CountryCode, language FROM countrylanguage WHERE IsOfficial = 'T' AND Percentage >= 98.0;

DROP INDEX IF EXISTS idx_countrylanguage ON countrylanguage;

CREATE INDEX idx_countrylanguage ON countrylanguage(IsOfficial, Percentage);
```

## 3

```sql
SELECT Name, Continent, Population FROM country WHERE SurfaceArea <= 200;

EXPLAIN ANALYZE SELECT Name, Continent, Population FROM country WHERE SurfaceArea <= 200;

DROP INDEX IF EXISTS idx_country_covering ON country;

CREATE INDEX idx_country_covering ON country(SurfaceArea, Name, Continent, Population);
```

## 4

```sql
SELECT co.Name AS country_name, ci.Name AS capital FROM country AS co INNER JOIN city AS ci ON co.Capital = ci.ID WHERE Region = 'Eastern Asia' ORDER BY co.Name;

EXPLAIN ANALYZE SELECT co.Name AS country_name, ci.Name AS capital FROM country AS co INNER JOIN city AS ci ON co.Capital = ci.ID WHERE Region = 'Eastern Asia' ORDER BY co.Name;

DROP INDEX IF EXISTS idx_counrty_city ON country;

CREATE INDEX idx_counrty_city on country(Region, Capital);
```
