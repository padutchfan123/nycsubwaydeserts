---
layout: home
title: Home
---

# NYC Subway Deserts

This is the homepage.

Code chunk example:

```sql
UPDATE "sd_MapPLUTO" p
SET nearest_station_id = sub.subway_id
FROM (
    SELECT 
        p.id AS parcel_id,
        s.id AS subway_id
    FROM "sd_MapPLUTO" p
    JOIN LATERAL (
        SELECT 
            s.id,
            s.quality
        FROM "sd_subway_ee" s
        WHERE ST_DWithin(p.centroid_geom, s.geom, 1320)
        ORDER BY 
            p.centroid_geom <-> s.geom,
            s.quality DESC
        LIMIT 1
    ) s ON true
) sub
WHERE p.id = sub.parcel_id;
```