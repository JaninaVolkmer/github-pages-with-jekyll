---
title: "Difference between .values() and .values_list()"
date: 2022-03-01
---

.values() and .values_list() translate to a GROUP BY query. 
Means: the rows with duplicate values will be grouped into a single value.
Example: model "People" with the data:
+-----+--------+-----+
| id  |  name  | age |
+-----+--------+-----+
| 1   |   Nina | 39  |
| 2   |  Henk  | 14  |
| 3   |  Henk  | 38  |
| 4   |  Pluk  | 11  |
+-----+--------+-----+
: People.objects.values_list('name', flat=True) will return 3 rows: ['Nina', 'Henk', 'Pluk'] -> Henk is grouped into a single value
: People.objects.all() will return 4 rows
-> useful when using annotations:
People.objects.values_list('name', Sum('age')) -> will return:

+-------+---------+
|  name | age_sum |
+-------+---------+
|  Nina |   39    |
|  Henk |   52    |
|  Pluk |   11    |
+-------+---------+

-> age of Henk has been summed, and returned in a single row
-> .distinct() if different -> it only applies after the annotations
https://docs.djangoproject.com/en/4.0/ref/models/querysets/#values
