# Park Atlas Open Data

Cleaned, joined datasets covering the U.S. National Park System's 474 units,
extracted from official federal sources and refreshed nightly.
This repository snapshot: data retrieved September 05, 2026.

## Files and columns

### parks.csv (one row per park unit)
| Column | Meaning |
|---|---|
| `park_code` | 4-letter NPS park code (joins all files; also the URL slug at theparkatlas.com/parks/{code}/) |
| `name` | Full official unit name |
| `designation` | Official designation (National Park, National Monument, ...) |
| `unit_type` | Grouped unit type |
| `states` | Pipe-separated US state codes |
| `visitors_2025` | 2025 recreation visits from NPS visitor use statistics; empty when the unit does not report |
| `dark_sky_certified` | True when DarkSky International certifies the unit |
| `timed_entry_required` | True when the NPS feed currently flags a timed-entry or reservation requirement |
| `has_entrance_fees` | True when the unit publishes structured entrance fees |
| `bird_species` | Bird species on the official NPSpecies checklist; empty when none published |
| `alerts_active` | Active NPS alerts at snapshot time |

### entrance_fees.csv (one row per published fee)
| Column | Meaning |
|---|---|
| `park_code`, `park_name` | Unit identifiers |
| `fee_type` | Official fee title (private vehicle, motorcycle, per person, ...) |
| `cost_usd` | Cost in US dollars |
| `description` | Official fee description, truncated to 200 chars |

Parks absent from this file publish no structured entrance fee. That is not
proof of free entry; some units describe fees only in prose.

### campgrounds.csv (one row per NPS-listed campground)
| Column | Meaning |
|---|---|
| `park_code` | Parent unit |
| `campground` | Campground name |
| `total_sites` | Total campsites |
| `reservable` | True when bookable on Recreation.gov |
| `amenities` | Pipe-separated amenity tags |

### wildlife_checklist_counts.csv (one row per park with a checklist)
| Column | Meaning |
|---|---|
| `park_code`, `park_name` | Unit identifiers |
| `bird_species`, `mammal_species` | Species totals on the official NPSpecies checklists |

## Refresh cadence and changelog

A GitHub Actions job on theparkatlas.com refreshes the underlying federal data
nightly, republishes these CSVs at https://theparkatlas.com/data/, and this
repository syncs from those published files on its own nightly schedule
(`.github/workflows/sync.yml`). The commit log is the changelog: one commit per
day the data actually changed. `SNAPSHOT.txt` carries the retrieval date.

## Sources and license

**Sources:** National Park Service API, NPS visitation statistics (IRMA),
NPSpecies, Recreation.gov RIDB, DarkSky International certifications.
Public-domain federal data; this compilation is dedicated to the public domain
under [CC0 1.0](LICENSE). Attribution appreciated but not required:
[Park Atlas](https://theparkatlas.com).

Live, browsable versions of everything here: https://theparkatlas.com
Questions or corrections: [open an issue](https://github.com/kirkwood-justin/park-atlas-data/issues).
