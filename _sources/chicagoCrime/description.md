## Chicago Crime Record

Describes a single reported crime incident from the City of Chicago
[Crimes - 2001 to Present](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8t2)
dataset. The I-GUIDE Platform publishes yearly extracts of it; this building block was created for
[Chicago Crime data 2026](https://platform.i-guide.io/datasets/265e6957-7ecf-48e0-a179-b498533eb627)
(source: Chicago Data Portal, contributed by Yunfan Kang), but it applies to any year.

### Data access

```python
from urllib.request import urlretrieve
urlretrieve("https://storage.i-guide.io/datasets/265e6957-7ecf-48e0-a179-b498533eb627/export.csv")
```

### Mapping from the CSV export

The CSV export stores every value as a quoted string. This building block defines a typed JSON
representation whose property names match the Chicago Data Portal API field names:

| CSV column | Property | Type |
|---|---|---|
| ID | `id` | string (digits) |
| Case Number | `case_number` | string |
| Date | `date` | ISO 8601 local date-time |
| Block | `block` | string |
| IUCR | `iucr` | string (4 characters) |
| Primary Type | `primary_type` | string |
| Description | `description` | string |
| Location Description | `location_description` | string or null |
| Arrest | `arrest` | boolean |
| Domestic | `domestic` | boolean |
| Beat | `beat` | string (4 digits, leading zeros kept) |
| District | `district` | string (3 digits) or null |
| Ward | `ward` | integer 1–50 or null |
| Community Area | `community_area` | integer 1–77 or null |
| FBI Code | `fbi_code` | string (e.g. `08B`) |
| X Coordinate / Y Coordinate | `x_coordinate` / `y_coordinate` | number (EPSG:3435, US feet) or null |
| Year | `year` | integer |
| Updated On | `updated_on` | ISO 8601 local date-time |
| Latitude / Longitude | `latitude` / `longitude` | number (WGS 84) or null |
| Location (WKT) | `location` | GeoJSON Point or null |

Date-times are Chicago local time and carry no UTC offset. Locations are shifted from the actual
address for privacy, but stay on the same block.

### Semantics and validation

The JSON-LD context maps identifiers and timestamps to Dublin Core terms, latitude/longitude to the
W3C WGS 84 vocabulary, the location to GeoSPARQL `geo:hasGeometry`, and the remaining fields to a
dataset-specific namespace (`https://platform.i-guide.io/def/chicago-crime/`). SHACL rules check that
each record has a single identifier and case number, and that coordinates fall within Chicago's
bounding box.
