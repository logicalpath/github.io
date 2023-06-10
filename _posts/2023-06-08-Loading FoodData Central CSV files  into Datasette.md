---
layout: post
title: "Loading Food Data Central into Datasette"
---

I'm building a local database of food and nutrients contained in the food for personal use. I've decided to use the USDA FoodData Central data from the Foundations Foods [database](https://fdc.nal.usda.gov/download-datasets.html). I'm loading the data into [Datasette](https://datasette.io/) to explore the data.

I used [sqlite-utils](https://sqlite-utils.datasette.io/en/stable/cli.html) to load csv files into the database:


```bash
sqlite-utils insert ../db/myfood.db food ../csv/foundation/food.csv --csv --detect-types
```

```bash
sqlite-utils insert ../db/myfood.db food_nutrient ../csv/foundation/food_nutrient.csv --csv --detect-types
```

```bash
sqlite-utils insert ../db/myfood.db nutrient ../csv/foundation/nutrient.csv --csv --detect-types
```

Use Datasette to view the database:

```bash
(Food) √ data > datasette db/myfood.db                                     main
INFO:     Started server process [8137]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8001 (Press CTRL+C to quit)
```

In the browser, I can view the tables in the database. Looking at the food table, I see the fdc_id which is referenced in the food_nutrient and nutrient tables and used to find the nutrients found in each food. I see that HUMUS is listed multiple times under the same data_type, each with a different fdc_id.

![food]({{site.url}}/images/datasette-food.png)

My expectation was that the fdc_id would be unique in this table. So I asked chatGPT how to determine that and it came up with a better approach than my idea by using GROUP BY and HAVING:

![unique]({{site.url}}/images/fdc_id-unique.png)
	

Zero results indicates that there are no duplicate results, so the values are unique. The next thing I'm wondering is if I search for 'Humus ' in the food table, what will I find? Datasette makes it very convenient to search a table. I use drop-downs to view:
rows where the description column contains 'Humus'
and I see 151 rows returned! That's a lot of Humus.

![humus]({{site.url}}/images/lot-of-humus.png)


Humus is found with different data_types. When I order by data_type, I find that there are several data_types listed:
- foundation_food
- market_acquisition
- sample_food
- sub_sample_food

Each data_type refers to a separate table. I wonder what data_types occur in the food table. Datasette provides the ability to [facet](https://docs.datasette.io/en/stable/facets.html) by column. If I facet the food table by the data_type column, I see that there are 5 data_type values:

![faceted]({{site.url}}/images/data_type_faceted.png)


I need to understand what these data_types mean so that I can decide how to best find the nutrient information for a specific food. I will leave that for another blog post. Thank you for taking the time to read this post!
