# Semantic Layer

Semantic layer is like a translation layer between data and business users. 
It allows business users to use data to build actionable insights without needing to understand the underlying complexity.

## Difference from other similar data layers
Just like Metrics layer, Metrics store and Headless BI, Semantic layer serves as a centralised place for business metrics to remove data silos and improve 
data governance by being a single source of truth. And these terms get used interchangeable often.

However, Semantic layer is more about giving data its meaning. While staying on top of a data platform, it provides context beyond just showing numbers or codes.

## How Semantic layer serves
Semantic layer exposes metrics in the form of measures and dimensions to the end users as a consistent abstraction of the data. 
Behind this abstraction, the Semantic layer maps the metric definition to the source data or data model, and transforms/joins data at a query time dynamically.  

A logical model in the Semantic layer typically contains measures and dimensions defined by configuration files (e.g., YAML). 
Dimensions are the columns for users to slice, dice, group and filter by (e.g., Categories). And Measure are the quantitative values to aggregate. 
Leveraging the logical model, users can be analyse metrics with flexibility as compared to working with static pre-aggregated metrics.

## Semantic layer tools
There are many tools in the market that can help data team build Semantic layers. Some popular options are: dbt cloud and Cube.

These tools not only allows the data team to build the Semantic layer with consolidated metrics with context, 
but also provide many other features like APIs, caching, access control and so on.

## Pros and Cons
- Pros: Easy drag & drop usage of metrics on data applications. Flexibility. Single source of truth.
- Cons: Engineering overhead to maintain and publish new metrics. Dependence on data team to build metrics.
