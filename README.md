# presidential-precinct-map-2024
# Presidential precinct data for the 2024 U.S. general election

The New York Times scraped and standardized precinct-level election results from around the country, and joined this tabular data to GIS precinct data to create a detailed nationwide map. This map is still a work in progress as we work to collect and standardize data from more states and counties, and some places are not expected to publish precinct-level data. The data in this repo will be updated occasionally along with our map.

The GeoJSON dataset can be downloaded here: TKlink to file

Each precinct polygon has the following properties:

- `GEOID`: unique identifier for the precinct, formed from the five-digit county FIPS code followed by the precinct name/ID (eg, 30003-08 or 39091-WEST MANSFIELD)
- `votes_dem`: votes received by Kamala Harris
- `votes_rep`: votes received by Donald Trump
- `votes_total`: total votes in the precinct, including for third-party candidates and write-ins when available
- `pct_dem_lead`: (`votes_dem` - `votes_rep`) / (`votes_total`), rounded to one decimal place (eg, -21.3)
- `official_boundary`: `true` if the precinct’s shape came from a file that provided by the state or county, `false` if the precinct boundary was estimated (see caveats below)

The 2020 election results that appear in our interactive map are from the [Voting and Election Science Team](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/K7760H). Shift in margin from 2020 was calculated by reallocating 2020 precinct results into 2024 precinct shapes [using this method](https://medium.com/@DeniseDSLu/dasymetric-reaggregation-using-mapshaper-218e87babaa3). Some 2020 data is omitted from our map because of methodological differences between the 2020 and 2024 datasets.

Please contact [electiondata@nytimes.com](mailto:electiondata@nytimes.com) if you have any questions about data quality or sourcing, beyond the caveats we describe below.

## General caveats

- We used official precinct boundaries provided by the states or counties for most of the places in our map. When official boundaries were not available, we estimated precinct shapes ourselves using points in the [voter files by L2](https://www.l2-data.com), a nonpartisan voter data vendor. This results in _generally accurate_ precinct boundaries, but can be rough in no- or very-low-population places.
  - Because of this, spatially joining the precincts in our GeoJSON without official boundaries will likely yield less-than-ideal output.
- In the following states, precinct results in some counties were not included in the map because these counties reported absentee votes at the countywide level rather than the precinct level: Idaho, Michigan, Missouri, North Carolina, Oklahoma and South Dakota.
- Some of the results we gathered are slightly incomplete:
  - Wherever write-ins are not reported by the data source, our vote totals are marginally different.
  - A very small portion of the tabular precinct results (roughly TK%) could not be joined to the precinct boundaries, and thus these results are not present in the GeoJSON.
- Our map uses township-level data in much of New England. We will replace it with precinct-level data as it becomes available.
- A few areas, such as TK, contain no voters, and those polygons are excluded from the GeoJSON.

## State-by-state data availability and caveats

|symbol|meaning|
|:----:|:------|
|✅|have gathered data, no significant caveats|
|⚠️|have gathered data, but doesn't cover entire state or has other significant caveats|
|❌|precinct data not usable|

One of the most common causes of precinct data being unusable is “countywide” tabulations. This occurs when a county reports, say, all of its absentee ballots together as a single total (instead of precinct-by-precinct); because we can’t attribute those ballots to specific precincts, that means that all precincts in the county will be missing an indeterminate number of votes, and therefore can’t be reliably mapped. In these cases, we drop the entire county from our GeoJSON.

| symbol | meaning                                                                             |
| :----: | :---------------------------------------------------------------------------------- |
|   ✅   | have gathered data, no significant caveats                                          |
|   ⚠️   | have gathered data, but doesn't cover entire state or has other significant caveats |
|   ❌   | precinct data not usable                                                            |


One of the most common causes of precinct data being unusable is “countywide” tabulations. This occurs when a county reports, say, all of its absentee ballots together as a single total (instead of precinct-by-precinct); because we can’t attribute those ballots to specific precincts, that means that all precincts in the county will be missing an indeterminate number of votes, and therefore can’t be reliably mapped. In these cases, we drop the entire county from our GeoJSON.

|                                                                                    state                                                                                     | availability | description                                                                                           | shapefile sourcing                                       |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :----------: | :---------------------------------------------------------------------------------------------------- | :------------------------------------------------------- |
|                                                                                   Alabama                                                                                    |      ❌      | absentee and provisional votes are reported countywide                                                | N/A                                                      |
|                                                                                    Alaska                                                                                    |      ❌      | absentee, early and provisional votes are reported district-wide                                      | N/A                                                      |
|                                                                                   [Arizona](https://azsos.gov/elections/election-information/2024-election-info)                                                                                    |      ✅      |                                                                                                       | official boundaries for all counties except La Paz       |
|                                                                                   [Akansas](https://results.enr.clarityelections.com/AR/122502/web.345435/#/reporting)                                                                                    |      ⚠️      | see notes below                                                                                       | some boundaries generated                                |
|                                                                                  California                                                                                  |      ⚠️      | data for some counties has not yet been collected by The Times                                                    | official boundaries                                      |
|                                                                                   Colorado                                                                                   |      ❌      | data is available but has not yet been collected by The Times                                                      | N/A                                                      |
|                                                                                 Connecticut                                                                                  |      ⚠️      | results are shown at the township level                                                               | official boundaries                                      |
|                                            [Delaware](https://elections.delaware.gov/results/html/index.shtml?electionId=GE2024)                                             |      ⚠️      | data is available, but has not yet been collected by The Times                                        | N/A                                                      |
|                                                                             [District of Columbia](https://electionresults.dcboe.org/election_results/2024-General-Election)                                                                             |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                   Florida                                                                                    |      ⚠️      | data for some counties has not yet been collected by The Times                                                    | some boundaries generated                                |
|                                                                                   [Georgia](https://results.sos.ga.gov/results/public/Georgia/elections/2024NovGen)                                                                                    |      ✅      | some boundaries generated                                                                             |
|                                                           [Hawaii](https://elections.hawaii.gov/election-results/)                                                           |      ❌      | data is available but has not yet been collected                                                      | N/A                                                      |
|                                                                                    [Idaho](https://voteidaho.gov/election-results/)                                                                                     |      ⚠️      | results are not shown for some counties where absentee votes are reported countywide | generated boundaries                                     |
|                                [Illinois](https://www.elections.il.gov/electionoperations/ElectionVoteTotalsPrecinct.aspx?ID=9huvqbsiUWA%3d)                                 |      ⚠️      | data for some counties has not yet been collected by The Times                                                     | official boundaries                                      |
|                                                                                   [Indiana](https://github.com/openelections/openelections-data-in/tree/master/2024/counties)                                                                                    |      ❌      | data is available but has not yet been collected by The Times                                                      | N/A                                                      |
|                                                                                     [Iowa](https://electionresults.iowa.gov/IA/122322/web.345435/#/reporting)                                                         |      ✅      | data is available but has not yet been collected by The Times                                                      | official boundaries                                      |
|                                                                                   [Kentucky](https://elect.ky.gov/results/2020-2029/Pages/2024.aspx)                                                                                   |      ✅      |                                                                                                       | generated boundaries                                     |
|                                                                                  Louisiana                                                                                   |      ❌      | absentee, early and provisional votes are reported by Parish, not by precinct                         | N/A                                                      |
|                                                                                    Maine                                                                                     |      ⚠️      | results are shown at the township level                                                               | official boundaries                                      |
|                                                                                   Maryland                                                                                   |      ✅      |                                                                                                       | some generated boundaries                                |
|                                                                                Massachusetts                                                                                 |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                   Michigan                                                                                   |      ✅      | see notes below                                                                                       | official boundaries, with modifications. see notes below |
|                                                                                  Minnesota                                                                                   |      ✅      |                                                                                                       | official boundaries, with modifications                  |
|                                       [Mississippi](https://github.com/openelections/openelections-data-ms/tree/master/2024/counties)                                        |      ❌      | data is available but has not yet been collected                                                      | N/A                                                      |
|                                         [Missouri](https://github.com/openelections/openelections-data-ms/tree/master/2024/counties)                                         |      ⚠️      | data is available for some counties but has not yet been collected by The Times                  | official boundaries                                      |
|                                                                                   Montana                                                                                    |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                   Nebraska                                                                                   |      ✅      |                                                                                                       | generated boundaries                                     |
|                                                                                    Nevada                                                                                    |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                New Hampshire                                                                                 |      ⚠️      | results are shown at the township level                                                               | official boundaries                                      |
|                                                                                  New Jersey                                                                                  |      ⚠️      | data for some counties has not yet been collected by The Times                                                     | some generated boundaries                                |
|                                                                                  New Mexico                                                                                  |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                   New York                                                                                   |      ⚠️      | data for some counties has not yet been collected by The Times                                                     | official boundaries                                      |
|                                    [North Carolina](https://www.ncsbe.gov/results-data/election-results/historical-election-results-data)                                    |      ✅      | results are not shown for some counties where absentee votes are reported countywide | official boundaries                                      |
| [North Dakota](<[https://www.ncsbe.gov/results-data/election-results/historical-election-results-data](https://results.sos.nd.gov/ResultsSW.aspx?text=All&type=SW&map=CTY)>) |      ❌      | data is available but has not yet been collected by The Times                                                      | N/A                                                      |
|                                     [Ohio](https://www.ohiosos.gov/elections/election-results-and-data/2024-official-election-results/)                                      |      ❌      | data for some counties has not yet been collected by The Times                                                     | some generated boundaries                                |
|Oklahoma|⚠️|results are not shown for some counties where absentee votes are reported countywide|official boundaries
|Oregon|⚠️|data for some counties has not yet been collected by The Times|generated boundaries
|Pennsylvania|⚠️|data for some counties has not yet been collected by The Times|some generated boundaries
|Rhode Island|⚠️|results are shown at the township level|official boundaries
|South Carolina|✅||official boundaries
|[South Dakota](https://electionresults.sd.gov/resultsSW.aspx?type=SWR&map=CTY)|⚠️|data is available for some counties but has not yet been collected by The Times|official boundaries
|Tennessee|✅||official boundaries
|Texas|⚠️|data for some counties has not yet been collected by The Times|some generated boundaries
|Utah|✅||
|Vermont|⚠️|results are shown at the township level|official boundaries
|Virginia|✅|see notes below|official boundaries
|Washington|✅||official boundaries
|[West Virginia](https://results.enr.clarityelections.com/WV/Berkeley/122768/web.345435/#/summary?v=355380%2F)|❌|data is available but has not yet been collected by The Times|official boundaries
|[Wisconsin](https://github.com/jdjohn215/wisc-election-night-data/tree/main/2024-nov/wec)|✅||official boundaries
|[Wyoming](https://sos.wyo.gov/Elections/Docs/2024/2024GeneralResults.aspx)|❌|data is available but has not yet been collected by The Times|N/A

## Additional data notes

- In Virginia, provisional votes for each candidate were reported at the county level rather than the precinct level. The Times allocated these votes to precincts according to each candidate’s share of the precinct-level reported vote.
- In Michigan, the city of Detroit reports its absentee votes in counting boards, which often span multiple precincts. For the 2024 data, The Times obtained a list of precincts that correspond to each counting board from the Detroit City Clerk, and precinct results were aggregated into counting boards. For 2020, the list of precincts that correspond to each counting board was obtained from OpenElections. In Ionia County, votes were reported at the township level rather than the precinct level, and that is what is shown on the map.
- In Arkansas, precinct results for Phillips County were unable to be joined to geographic shapes.

## Credits

- Map by [Saurabh Datar](https://www.nytimes.com/by/saurabh-datar), [Alex Lemonides](https://www.nytimes.com/by/alex-lemonides), [Ilana Marcus](https://www.nytimes.com/by/ilana-marcus), [Eli Murray](https://www.nytimes.com/by/eli-murray), [Ethan Singer](https://www.nytimes.com/by/ethan-singer) and [Christine Zhang](https://www.nytimes.com/by/christine-zhang).
- Data collection and additional contributions by [Crystal Arroyo](https://www.nytimes.com/by/crystal-arroyo), [Ademola Bello](https://www.nytimes.com/by/ademola-bello), [Aatish Bhatia](https://www.nytimes.com/by/aatish-bhatia), Matthew Berkowitz, Anna Bialas, [Irineo Cabreros](https://www.nytimes.com/by/irineo-cabreros), [Agnes Chang](https://www.nytimes.com/by/agnes-chang), [William B. Davis](https://www.nytimes.com/by/william-b-davis), Avery Dews, [Hang Do Thi Duc](https://www.nytimes.com/by/hang-do-thi-duc), [Madison Dong](https://www.nytimes.com/by/madison-dong), Will Dunning, [Andrew Fischer](https://www.nytimes.com/by/andrew-fischer), [Tom Giratikanon](https://www.nytimes.com/by/tom-giratikanon), Angela Guo, [Rebecca Halleck](https://www.nytimes.com/by/rebecca-halleck), [Samuel Jacoby](https://www.nytimes.com/by/samuel-jacoby), Laura Bejder Jensen, Yuchen Jin, [Aaron Krolik](https://www.nytimes.com/by/aaron-krolik), Branden Lawrence, Leilani Leach, [Joey Lee](https://www.nytimes.com/by/joey-k--lee), [Bea Malsky](https://www.nytimes.com/by/bea-malsky), [Blacki Migliozzi](https://www.nytimes.com/by/blacki-migliozzi), Katherine Oung, [Francesca Paris](https://www.nytimes.com/by/francesca-paris), Eric Rabinowitz, [Mira Rojanasakul](https://www.nytimes.com/by/mira-rojanasakul), [Isabel Sieh](https://www.nytimes.com/by/isabel-sieh), [Albert Sun](https://www.nytimes.com/by/albert-sun), [Rumsey Taylor](https://www.nytimes.com/by/rumsey-taylor), Bella Virgilio, [Eve Washington](https://www.nytimes.com/by/eve-washington) and Miles Watkins.
- Precinct results for several counties in Michigan provided by [Derek Willis](https://thescoop.org) and [the OpenElections team](https://github.com/openelections/openelections-data-mi/graphs/contributors).
- Precinct results and boundaries for New York provided by [Benjamin J. Rosenblatt](https://www.benjrosenblatt.com). The Times obtained results for Rockland County from the [county results page](https://app.enhancedvoting.com/results/public/rockland-county-ny/elections/GE2024Results).
- Precinct results and boundaries for Wisconsin provided by [John D. Johnson](https://johndjohnson.info).
- [Josh Metcalf](https://x.com/josh_metcalf) provided assistance in matching boundaries for Nevada, New Hampshire, Mississippi, Vermont, and Wyoming.
- Township election results for Connecticut, Maine, New Hampshire, Rhode Island, and Vermont provided by the Associated Press.
