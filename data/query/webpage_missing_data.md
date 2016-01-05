# Query: Webpage Missing Data

**Webpage Missing Data** is a query that returns list of webpage documents
missing important data. Important data is: `sys_title`, `sys_description` or
`sys_content_plaintext`.

It also returns aggregation statistics about total count of missing docs per
individual fields for each content provider.

## URL parameters

No URL parameters accepted.

**Example:**

- <http://dcp_server:port/v2/rest/search/webpage_missing_data>

## Query output format

Up to 50 first matching documents are returned in `hits.hits[]`. Every returned document contains
important fields (see above for the list) and `check#` fields with missing flag. This way it is easy
to learn which particular fields were identified as missing for individual documents.

The top aggregation section `aggregations.sys_content_provider.buckets[]` provides statistics per individual content
providers (see the `key` value for content provider code). 

For each content provider the `sys_content_type.buckets[]` contain statistics about missing important data in documents
(see the `key` value for content type code). At this level there are provided `missing#` counts for each missing
important field.

For example:

The following output means there were found `128` documents for content provider `rht` missing important data.
All of those documents belong to `rht_website` content type. In these documents `25` are missing `sys_title` field,
`22` are missing `sys_content_plaintext` and finally `128` are missing `sys_description` field.

````
  "aggregations": {
    "sys_content_provider": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "rht",
          "doc_count": 128,
          "sys_content_type": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "rht_website",
                "doc_count": 128,
                "missing#sys_title": {
                  "doc_count": 25
                },
                "missing#sys_content_plaintext": {
                  "doc_count": 22
                },
                "missing#sys_description": {
                  "doc_count": 128
                }
              }
            ]
          }
        }
      ]
    }
  }
````

## Query implementation details

Source of the query can be found in [`webpage_missing_data.json`](webpage_missing_data.json) file.
It is not string-encoded ATM thus the source is easily readable.