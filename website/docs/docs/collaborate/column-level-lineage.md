---
title: "Column level lineage"
description: "dbt Explorer provides recommendations that you can take to improve the quality of your dbt project."
---

dbt Explorer now offers column level lineage (CLL) for the resources in your dbt project. Analytics engineers can quickly and easily gain insight into the provenance of their data products at a more granular level. For each column in a dbt resource (model, source, or snapshot), Explorer provides the full lineage for the data in that column.

Column level lineage is available to dbt Cloud Enterprise accounts that can use Explorer. It’s also available through the Discovery API.

:::tip Beta
Column-level lineage is now available in beta. Check it out! We'd love to [know what you think](https://docs.google.com/forms/d/e/1FAIpQLSdpCbVkGY9QwfExFonpWE4DTOKi3fQxBGLD0wwKYpkMjgcE7g/viewform)!
:::

## No setup required

There is no additional setup required for column level lineage if your account is on an Enterprise plan that can use Explorer. You can access column level lineage by expanding the column card in the **Columns** tab of an Explorer [resource details page](/docs/collaborate/explore-projects#view-resource-details) for a model, source, or snapshot.

Lineage will update after each run executed in the production environment. Make sure that `docs generate` is running within at least one job in the environment. Refer to [Generating metadata](/docs/collaborate/explore-projects#generate-metadata) for more details.

<Lightbox src="/img/docs/collaborate/dbt-explorer/example-cll.png" title="Example of the Columns tab and where to expand for the CLL"/>

<LoomVideo id='278c948ba387457884cc6b9545793685' />

## What you can use column level lineage for {#use-cases} 

Click on these tabs to learn more about the CLL use cases, the analysis you can do, and the results you can achieve:

<Tabs>
<TabItem value="root-cause" label="Root cause analysis">

When there is an unexpected breakage in a data pipeline, column level lineage can be a valuable tool to understand the exact point in the pipeline where the error took place. For example, a failing data test on a particular column in your dbt model might've stemmed from an untested column upstream. Using CLL can help quickly identify and fix breakages when they happen.

</TabItem>
<TabItem value="impact" label="Impact analysis">

During development, analytics engineers can use column level lineage to understand the full scope of the impact of their proposed changes. This knowledge empowers them to create higher quality pull requests that require less rework, as they can anticipate and preempt issues that would've been unchecked without column level insights. 

</TabItem>
<TabItem value="collaboration" label="Collaboration and efficiency">

When exploring your data products, navigating column lineage allows analytics engineers and data analysts to more easily navigate and understand the origin and usage of their data, enabling them to make better decisions with higher confidence.
</TabItem>
</Tabs>

## Caveats

Column level lineage relies on SQL parsing. Errors can occur when parsing fails or a column's origin is unknown (like with JSON unpacking, lateral joins, and so forth). In these cases, lineage may be incomplete and dbt Cloud will provide a warning about it in the column lineage. To review the error details, open the [full lineage graph](/docs/collaborate/explore-projects#project-lineage) and select the node to open the column’s details panel. 

<Lightbox src="/img/docs/collaborate/dbt-explorer/example-parsing-error-pill.png" width="90%" title="Example of warning in the full lineage graph"/>

Possible error cases:

- **Parsing error** &mdash; Error occurs when the SQL is ambiguous or too complex for parsing. An example of ambiguous parsing scenarios are _complex_ lateral joins.
- **Python error** &mdash; Error occurs when a Python model is used within the lineage. Due to the nature of Python models, it's not possible to parse and determine the lineage.
- **Unknown error** &mdash; Error occurs when the lineage can't be determined for an unknown reason. An example of this would be if a dbt best practice is not being followed, like using hardcoded table names instead of `ref` statements.


