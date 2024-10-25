# Searching in Assemblyline

Assemblyline provides robust search capabilities within its user interface, allowing users to search for anything stored in its indices. By using the search widget, users can submit queries following the Lucene query syntax, which are then handled by the search engine. The fields available for searching are determined by several Object Data Models (ODMs) captured via Elasticsearch indices.

## Understanding Indices

Elasticsearch indices enable Assemblyline to deduplicate most of the results in the system, which significantly enhances its scalability. Searching through indexed fields is also remarkably fast.

Assemblyline has six primary indices:

- **Alert**
- **File**
- **Result**
- **Retrohunt**
- **Signature**
- **Submission**

You can view all indices and their indexed fields from the `Help > Search Help` menu in your Assemblyline installation.

## Using the Search Interface

### Search Bar

The search bar, located at the top of the user interface, lets you perform searches across all indices.

![A search bar interface, part of the Assemblyline user interface. The search bar is located on a dark background and features a magnifying glass icon on the left side, indicating its function for searching. To the right of the search bar, there are three icons: one for keyboard shortcuts (CTRL+K), another for notifications with a number “12” suggesting there are 12 notifications, and an icon representing the avatar of the logged in user.](./images/search_bar.png){: .center }

### Search Page

Additionally, you can perform searches using the generic Search page.

![A dark-themed search page interface with the word ‘Search’ at the top in white text. Below it, there is a search bar with rounded corners and lighter grey shade. The placeholder text inside the search bar reads ‘Search all indexes…’ in grey. On the right side of the search bar, there is a magnifying glass icon for search and an ‘x’ icon to clear the input field.](./images/search_view.png)

### Search Results

Search results will be displayed across the different indices. The results are categorized by:

- **SUBMISSION**
- **FILE**
- **RESULT**
- **SIGNATURE**
- **ALERT**
- **RETROHUNT**

![A screenshot of a search interface with the word ‘blah’ typed into the search bar. Below the search bar are tabs labeled SUBMISSION, FILE, RESULT, SIGNATURE, ALERT, and RETROHUNT representing various indices, all showing (0) entries. A message box below the tabs states ‘No submissions found!’ and another line reads ‘The query that you ran did not return any results.](./images/searching_across_indices.png)

!!! tip "You must limit your search criteria to a single index. Searching across multiple indices simultaneously (i.e., JOIN queries) is not supported."

This limitation can be mitigated by using the [Assemblyline Client](../../integration/python/) to perform queries on one index and then refine or enrich your search by querying another index.

## Search Examples

### Basic Searches

To familiarize yourself with the indices, use the "*Find related results*" option from the tags dropdown menu.

![Screenshot showing a dropdown menu with options including 'Copy to clipboard', 'Find related results', 'Toggle highlight', and 'Add to safelist'. The 'Find related results' option is highlighted. The dropdown menu can be accessed from right-clicking tags found in a submission.](./images/magnifier.png){: .center }

For example, clicking it on the `av.virus_name` tag (`HEUR/Macro.Downloader.MRAA.Gen`) will generate the following query:
```ruby
result.sections.tags.av.virus_name:"HEUR/Macro.Downloader.MRAA.Gen"
```

### Advanced Searches

Harness the full power of the [Lucene query syntax](https://www.elastic.co/guide/en/kibana/current/lucene-query.html) for more complex searches. Here are a few examples:

```ruby
# Find every result where the ViperMonkey service extracted the IP 10.10.10.10
result.sections.tags.network.static.ip:"10.10.10.10" AND response.service_name:ViperMonkey

# Find all submissions with a score greater than or equal to 2000 in the last two days
max_score:[2000 TO *] AND times.submitted:[now-2d TO now]

# Find all anti-virus results with Emotet in the signature name
result.sections.tags.av.virus_name:*Emotet*
```
Assemblyline supports various search parameters, including wildcards, ranges, and regex. Refer to `Help > Search Help` for comprehensive syntax.

Search queries can also be used with the [Assemblyline Client](../../integration/python) to automate complex tradecraft as new files are processed by the system.

## Autofill Feature

Given the wide range of searchable fields per index, the "autofill" feature can assist in constructing queries. To use autofill, navigate to an index-specific search page, such as "Result" (`/search/result`), and start typing. Autofill will suggest available fields:

![Autofill options on the Result search page](./images/autofill_options.png)

For instance, if you wish to query all submissions marked as `TLP:CLEAR` and containing a service that scored greater than 500, you should search within the Result index:

![Example query result](./images/example_query_result.png)
