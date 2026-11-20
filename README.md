# Governance Catalog

Governance Catalog is a planned open-source tool for data teams that need to understand how datasets fit together across systems. Teams will be able to register datasets, organise them into data products by domain, trace where a field came from and see which source fields describe the same business concept. A local web interface and HTTP API will expose the same catalog and access policies.

**Project status:** Design stage. The repository currently contains the project description and licence. Setup instructions will accompany the first runnable milestone.

## Financial services example

An analyst wants to group fictional issuers by region of incorporation. The issuer reference team owns a **Company Directory** data product backed by PostgreSQL. The geography team owns a **Country Directory** data product exposed through an HTTP API.

| Data product | Example fields | Synthetic example |
| --- | --- | --- |
| Company Directory | `company_id`, `company_name`, `incorporation_country_code` | `C001`, Alder Quay Example plc, `GB` |
| Country Directory | `country_code`, `country_name`, `region` | `GB`, United Kingdom, Europe |

The catalog will read the two schemas and suggest a mapping between the country-code fields. A steward will confirm that the company field means *country of incorporation*, a distinction that matters when a company has headquarters elsewhere. The conceptual model can then express **Company → incorporated in → Country**, with Country linked to its Region.

For the example issuer, the analyst should see *Europe* and be able to trace the result. Field lineage will show that the region value came from `Country Directory.region` and that the lookup matched `incorporation_country_code` to `country_code`. The demonstration will use fictional company records and local geography fixtures.

## Offline desktop design

The first release is planned around Python's standard library, a local SQLite catalog and browser assets shipped with the project. A local HTTP server will serve the UI and API. This design requires a desktop with Python 3, its SQLite module and a browser. It requires no Docker, internet connection, package downloads or hosted services.

| Source represented | Planned offline input |
| --- | --- |
| PostgreSQL | A synthetic Company Directory schema export listing columns, types and keys |
| HTTP API | A local OpenAPI JSON document describing the Country Directory response |

These files will let a reviewer exercise the catalog without installing database servers or calling a remote API. Catalog search will use SQLite, and the bundled UI will display lineage. The planned application has no runtime dependency on AI agents, model inference, SageMaker, Kibana or Elasticsearch. Concept suggestions will use explicit mapping rules and human review.

## Planned milestones

1. **Register and browse a dataset.** Import a synthetic PostgreSQL schema export, assign the dataset to a data product within a domain and inspect its fields through the UI and API.
2. **Connect concepts across sources.** Import the geography API schema and map its country code to the company schema. Link tags to glossary terms, review candidate field mappings and record relationships between approved concepts.
3. **Trace a derived field.** Record a source field, output field and transformation. Show upstream inputs and downstream dependants through the API and lineage view.
4. **Apply team access policies.** Give a sample team member read access to one data product. Verify that both the UI and direct API requests respect that boundary.
5. **Retrieve data through the conceptual model.** Use approved mappings to resolve a concept to fields in local synthetic datasets and return only values permitted by the caller's policy.

The first end-to-end demonstration will start with local metadata files and finish with a user browsing a permitted data product, following a lineage edge and finding a related concept in another domain. Live source connections are a future extension.

## Licence

[Apache License 2.0](LICENSE)
