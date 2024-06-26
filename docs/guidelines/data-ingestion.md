# Data Ingestion Guidelines

See [Data System architecture](../architecture/components/data-system.md)

Before we ingest any closed (private or sensitive) data, we **must** have a [Data Sharing Agreement](data-sharing-agreement.md) in place.

See also

- [Data Sharing Agreement](data-sharing-agreement.md)
- [Metadata Ingestion Guidelines](metadata-ingestion.md)

## We ingest data: minimum requirements

This contains the minimum requirements for data to be ingested into the RIMReP DMS.

It consists of

- [Data access](#data-access)
- [Data structure](#data-structure) (format, data types, ...)

### Data access

Data access requirements are categorised based on data update frequency.

Note: "direct" data access means zero human intervention (eg. no email to download forms, no pop-up to accept conditions before download)

#### Non-updated

- Manual handling of data is acceptable.
- We can manually upload original data files to our S3.
  - or provide a mechanism for data providers to upload the data to our S3 (gbr-dms-shared-access)

#### Infrequently updated (i.e. >=yearly)

- If "direct" download access is available - we will use it
- If not, download access that requires human intervention is acceptable (eg email form)
  - How we downloaded the file must be documented

#### Frequently updated data (< yearly)

- We need "direct" download access
  - This can be an API (that supports downloading the entire dataset) or file-based

#### Very frequently updated data (< weekly)

- We need "direct" download access
- For flat (non-chunked) datasets
  - For larger datasets (>1GB), this must be API access
  - For smaller datasets (<=1GB), file-based access is acceptable
- For chunked datasets (eg multi-netcdfs)
  - File-based chunks must be of reasonable granularity/size (eg daily NetCDFs)
  - Or, we need an adequate API

### Data structure

#### Formats

- Data formats should be well-accepted and supported, or open source, ideally both.
  - We shouldn't try to support arbitrary unknown proprietary formats.
- Must be machine readable (eg no PDFs, images, web scraping, ...)
- Must be tabular or multi-dimensional numerical data
  - That is, convertible to parquet or zarr

#### Variables

We must be able to identify the following variable types:

- string/text
- numerical
  - discrete and/or continuous
- date/times
  - timezone
- geometry
  - point/line/polygon
  - CRS

We will not allow variables of mixed-types. We can perform basic cleansing of variables - for example dropping values that do not fit the specified type.
