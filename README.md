# Presidential precinct data for the 2024 U.S. general election

The New York Times collected and standardized precinct-level election results from around the country, and joined this tabular data to G.I.S. precinct data to create a [detailed nationwide map](https://www.nytimes.com/interactive/2025/us/elections/2024-election-map-precinct-results.html). This map is still a work in progress as we continue to collect and standardize data from more states and counties, and some places are not expected to publish precinct-level data. The data in this repo will be updated occasionally along with our map.

The TopoJSON data set can be downloaded here: [https://int.nyt.com/newsgraphics/elections/map-data/2024/national/precincts-with-results.topojson.gz](https://int.nyt.com/newsgraphics/elections/map-data/2024/national/precincts-with-results.geojson.gz)

Each precinct polygon has the following properties:

- `GEOID`: unique identifier for the precinct, formed from the five-digit county F.I.P.S. code followed by the precinct name/ID (e.g., 30003-08 or 39091-WEST MANSFIELD)

- `votes_dem`: votes received by Kamala Harris

- `votes_rep`: votes received by Donald J. Trump

- `votes_total`: total votes in the precinct, including for third-party candidates and write-ins when available

- `pct_dem_lead`: (votes_dem - votes_rep) / (votes_total), with four significant digits (e.g., -0.2134)

- `official_boundary`: `true` if the precinct’s shape came from a file that was provided by the state or county, `false`if the precinct boundary was estimated (see caveats below)

The 2020 election results that appear in our interactive map are primarily from the [Voting and Election Science Team](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/K7760H) (see additional data notes below for exceptions.) Shift in margin from 2020 was calculated by reallocating 2020 precinct results into 2024 precinct shapes [using this method](https://medium.com/@DeniseDSLu/dasymetric-reaggregation-using-mapshaper-218e87babaa3). Some 2020 data is omitted from our map because of methodological differences between the 2020 and 2024 data sets.

Please contact <electionsdata@nytimes.com> if you have any questions about data quality or sourcing beyond the caveats we describe below.


## General caveats

- We used official precinct boundaries provided by states or counties for most of the places in our map. When official boundaries were not available, we created approximate precinct shapes using points in the [voter files by L2](https://www.l2-data.com), a nonpartisan voter data vendor. This results in _generally accurate_ precinct boundaries, but the shapes can be more rough in no- or very-low-population places.

  - Because of this, spatially joining the precincts in our GeoJSON without official boundaries is likely to yield less-than-ideal output.

- In the following states, precinct results in some counties were not included in the map because these counties reported absentee votes at the countywide level rather than at the precinct level: Idaho, Michigan, Missouri, North Carolina, Oklahoma and South Dakota.

- Some of the results we gathered are slightly incomplete:

  - Wherever write-ins are not reported by the data source, our vote totals are marginally different.

  - A very small portion of the tabular precinct results (roughly 0.1%) could not be joined to the precinct boundaries, and thus these results are not present in the GeoJSON.

- Our map uses township-level data in much of New England. We will replace it with precinct-level data as it becomes available.

- A small number of precincts contain no votes or have so few voters that their vote data is redacted by officials. Those polygons are excluded from the GeoJSON.


## State-by-state data availability and caveats

One of the most common causes of precinct data being unusable is “countywide” tabulations. This occurs when a county reports, say, all of its absentee ballots together as a single total (instead of precinct by precinct); because we can’t attribute those ballots to specific precincts, that means that all precincts in the county will be missing an indeterminate number of votes, and therefore can’t be reliably mapped. In these cases, we drop the entire county from our GeoJSON.

|symbol|meaning|
|:----:|:------|
|✅|have gathered data, no significant caveats|
|⚠️|have gathered data, but doesn't cover entire state or has other significant caveats|
|❌|precinct data not usable|

One of the most common causes of precinct data being unusable is “countywide” tabulations. This occurs when a county reports, say, all of its absentee ballots together as a single total (instead of precinct by precinct); because we can’t attribute those ballots to specific precincts, that means that all precincts in the county will be missing an indeterminate number of votes, and therefore can’t be reliably mapped. In these cases, we drop the entire county from our GeoJSON.

|                                                                                    state                                                                                     | availability | description                                                                                           | shapefile sourcing                                       |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------: | :---------------------------------------------------------------------------------------------------- | :------------------------------------------------------- |
|                                                                                   Alabama                                                                                    |      ❌      | absentee and provisional votes are reported countywide                                                | N/A                                                      |
|                                                                                    Alaska                                                                                    |      ❌      | absentee, early and provisional votes are reported district-wide                                      | N/A                                                      |
|                                                                                   [Arizona](https://azsos.gov/elections/election-information/2024-election-info)                                                                                    |      ✅      |                                                                                                       | official boundaries for all counties except La Paz       |
|                                                                                   [Arkansas](https://results.enr.clarityelections.com/AR/122502/web.345435/#/reporting)                                                                                    |      ⚠️      | see notes below                                                                                       | official boundaries                                |
|                                                                                  California                                                                                  |      ⚠️      | data for some counties has not yet been collected                                                    | official boundaries                                      |
|                                                                                   Colorado                                                                                   |      ⚠️      | data for counties other than Denver has not yet been collected                                                      | ✅                                                      |
|                                                                                 Connecticut                                                                                  |      ⚠️      | results are shown at the township level                                                               | official boundaries                                      |
|                                            [Delaware](https://elections.delaware.gov/results/html/index.shtml?electionId=GE2024)                                             |      ✅      || official boundaries                                                 |
|                                                                             [District of Columbia](https://electionresults.dcboe.org/election_results/2024-General-Election)                                                                             |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                   Florida                                                                                    |      ⚠️      | data for some counties has not yet been collected                                                    | some boundaries generated                                |
|                                                                                   [Georgia](https://results.sos.ga.gov/results/public/Georgia/elections/2024NovGen)                                                                                    |      ✅      | | some boundaries generated                                                                             |
|                                                           [Hawaii](https://elections.hawaii.gov/election-results/)                                                           |      ❌      | data is available but has not yet been collected                                                      | N/A                                                      |
|                                                                                    [Idaho](https://voteidaho.gov/election-results/)                                                                                     |      ⚠️      | results cannot be shown for some counties where absentee votes are reported countywide | generated boundaries                                     |
|                                [Illinois](https://www.elections.il.gov/electionoperations/ElectionVoteTotalsPrecinct.aspx?ID=9huvqbsiUWA%3d)                                 |      ⚠️      | data for some counties has not yet been collected                                                     | official boundaries                                      |
|                                                                                   [Indiana](https://github.com/openelections/openelections-data-in/tree/master/2024/counties)                                                                                    |      ❌      | data is available but has not yet been collected                                                      | N/A                                                      |
|                                                                                     [Iowa](https://electionresults.iowa.gov/IA/122322/web.345435/#/reporting)                                                         |      ✅      || official boundaries                                      |
|                                                                                   [Kansas](https://sos.ks.gov/elections/election-results.html)                                                                                  |      ❌      |  data is available but has not yet been collected                                                                                                     | N/A                                     |
|                                                                                   [Kentucky](https://elect.ky.gov/results/2020-2029/Pages/2024.aspx)                                                                                   |      ✅      |                                                                                                       | generated boundaries                                     |
|                                                                                  Louisiana                                                                                   |      ❌      | absentee, early and provisional votes are reported by Parish, not by precinct                         | N/A                                                      |
|                                                                                    Maine                                                                                     |      ⚠️      | results are shown at the township level                                                               | official boundaries                                      |
|                                                                                   [Maryland](https://elections.maryland.gov/elections/2024/election_data/index.html)                                                                                   |      ✅      |                                                                                                       | some generated boundaries                                |
|                                                                                [Massachusetts](https://electionstats.state.ma.us/elections/view/165300/)                                                                                 |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                   [Michigan](https://github.com/openelections/openelections-data-mi/blob/master/2024/20241105__mi__general__precinct.csv)                                                                                   |      ⚠️      | results cannot be shown for some counties where absentee votes are reported countywide                                                                                        | official boundaries, with modifications. see notes below |
|                                                                                  [Minnesota](https://electionresults.sos.mn.gov/20241105)                                                                                   |      ✅      |                                                                                                       | official boundaries                  |
|                                       [Mississippi](https://github.com/openelections/openelections-data-ms/tree/master/2024/counties)                                        |      ❌      | data is available but has not yet been collected                                                      | N/A                                                      |
|                                         Missouri                                        |      ⚠️      | data for counties other than St. Louis has not yet been collected. results cannot be shown for some counties where absentee votes are reported countywide| official boundaries                                      |
|                                                                                   [Montana](https://sosmt.gov/elections/results/)                                                                                    |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                   [Nebraska](https://electionresults.nebraska.gov/resultsCTY.aspx?type=PRS&rid=12465&osn=90&map=CTY)                                                                                   |      ✅      |                                                                                                       | generated boundaries                                     |
|                                                                                    [Nevada](https://www.nvsos.gov/sos/elections/election-information/precinct-level-results)                                                                                    |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                New Hampshire                                                                                 |      ⚠️      | results are shown at the township level                                                               | official boundaries                                      |
|                                                                                  New Jersey                                                                                  |      ⚠️      | data for some counties has not yet been collected                                                     | some generated boundaries                                |
|                                                                                  [New Mexico](https://electionresults.sos.nm.gov/resultsSW.aspx?type=FED&map=CTY)                                                                                  |      ✅      |                                                                                                       | official boundaries                                      |
|                                                                                   New York                                                                                   |      ⚠️      | data for some counties has not yet been collected                                                     | official boundaries                                      |
|                                    [North Carolina](https://www.ncsbe.gov/results-data/election-results/historical-election-results-data)                                    |      ⚠️     | results cannot be shown for some counties where absentee votes are reported countywide | official boundaries                                      |
| [North Dakota](<[https://www.ncsbe.gov/results-data/election-results/historical-election-results-data](https://results.sos.nd.gov/ResultsSW.aspx?text=All&type=SW&map=CTY)>) |      ❌      | data is available but has not yet been collected                                                      | N/A                                                      |
|                                     [Ohio](https://www.ohiosos.gov/elections/election-results-and-data/2024-official-election-results/)                                      |      ⚠️      | data for some counties has not yet been collected                                                     | some generated boundaries                                |
|[Oklahoma](https://results.okelections.gov/OKER/?elecDate=20241105)|⚠️|results cannot be shown for some counties where absentee votes are reported countywide|official boundaries
|Oregon|⚠️|data for some counties has not yet been collected|generated boundaries
|Pennsylvania|⚠️|data for some counties has not yet been collected|some generated boundaries
|Rhode Island|⚠️|results are shown at the township level|official boundaries
|[South Carolina](https://scvotes.gov/elections-statistics/election-results/)|✅||official boundaries
|[South Dakota](https://electionresults.sd.gov/resultsSW.aspx?type=SWR&map=CTY)|❌|data is available but has not yet been collected. results cannot be shown for some counties where absentee votes are reported countywide|N/A
|[Tennessee](https://sos.tn.gov/elections/results)|✅||official boundaries
|Texas|⚠️|data for some counties has not yet been collected|some generated boundaries
|[Utah](https://electionresults.utah.gov/results/public/utah/elections/general11052024/ballot-items/88e40000-968a-0c37-b0b0-08dce9374078)|✅||official boundaries
|Vermont|⚠️|results are shown at the township level|official boundaries
|[Virginia](https://enr.elections.virginia.gov/results/public/Virginia/elections/2024NovemberGeneral)|✅|see notes below|official boundaries
|[Washington](https://results.vote.wa.gov/results/20241105/export.html)|✅||official boundaries
|[West Virginia](https://results.enr.clarityelections.com/WV/122766/web.345435/#/reporting)|✅||official boundaries
|[Wisconsin](https://github.com/jdjohn215/wisc-election-night-data/tree/main/2024-nov/wec)|✅||official boundaries
|[Wyoming](https://sos.wyo.gov/Elections/Docs/2024/2024GeneralResults.aspx)|❌|data is available but has not yet been collected|N/A

## Additional data notes

- In Virginia, provisional votes for each candidate were reported at the county level rather than at the precinct level. The Times allocated these votes to precincts according to each candidate’s share of the precinct-level reported vote.
- In Michigan, the city of Detroit reports its absentee votes in counting boards, which often span multiple precincts. For the 2024 data, The Times obtained a list of precincts that correspond to each counting board from the Detroit city clerk, and precinct results were aggregated into counting boards. For 2020 data, The Times obtained precinct results from the Wayne County Clerk and aggregated Detroit’s votes into counting boards using a list of precincts from [OpenElections](https://github.com/openelections/openelections-sources-mi/tree/master/2020). In Ionia County, votes were reported at the township level rather than at the precinct level, and that is what is shown on the map.
- In Arkansas, precinct results for Phillips County could not be joined to geographic shapes.
- The 2020 election results for New York were provided by [Benjamin J. Rosenblatt](https://www.benjrosenblatt.com).  



## Credits

- Map produced by [Saurabh Datar](https://www.nytimes.com/by/saurabh-datar), [Alex Lemonides](https://www.nytimes.com/by/alex-lemonides), [Ilana Marcus](https://www.nytimes.com/by/ilana-marcus), [Eli Murray](https://www.nytimes.com/by/eli-murray), [Ethan Singer](https://www.nytimes.com/by/ethan-singer) and [Christine Zhang](https://www.nytimes.com/by/christine-zhang).
- Data collection and additional contributions by [Crystal Arroyo](https://www.nytimes.com/by/crystal-arroyo), [Ademola Bello](https://www.nytimes.com/by/ademola-bello), [Aatish Bhatia](https://www.nytimes.com/by/aatish-bhatia), Matthew Berkowitz, Anna Bialas, [Irineo Cabreros](https://www.nytimes.com/by/irineo-cabreros), [Agnes Chang](https://www.nytimes.com/by/agnes-chang), [William B. Davis](https://www.nytimes.com/by/william-b-davis), Avery Dews, [Hang Do Thi Duc](https://www.nytimes.com/by/hang-do-thi-duc), [Madison Dong](https://www.nytimes.com/by/madison-dong), Will Dunning, [Andrew Fischer](https://www.nytimes.com/by/andrew-fischer), [Tom Giratikanon](https://www.nytimes.com/by/tom-giratikanon), Angela Guo, [Rebecca Halleck](https://www.nytimes.com/by/rebecca-halleck), [Samuel Jacoby](https://www.nytimes.com/by/samuel-jacoby), Laura Bejder Jensen, Yuchen Jin, [Aaron Krolik](https://www.nytimes.com/by/aaron-krolik), Branden Lawrence, Leilani Leach, [Joey Lee](https://www.nytimes.com/by/joey-k--lee), [Bea Malsky](https://www.nytimes.com/by/bea-malsky), [Blacki Migliozzi](https://www.nytimes.com/by/blacki-migliozzi), Katherine Oung, [Francesca Paris](https://www.nytimes.com/by/francesca-paris), Eric Rabinowitz, [Mira Rojanasakul](https://www.nytimes.com/by/mira-rojanasakul), [Isabel Sieh](https://www.nytimes.com/by/isabel-sieh), [Albert Sun](https://www.nytimes.com/by/albert-sun), [Rumsey Taylor](https://www.nytimes.com/by/rumsey-taylor), Bella Virgilio, [Eve Washington](https://www.nytimes.com/by/eve-washington) and Miles Watkins.
- Precinct results for several counties in Michigan provided by [Derek Willis](https://thescoop.org) and [the OpenElections team](https://github.com/openelections/openelections-data-mi/graphs/contributors).
- Precinct results and boundaries for New York provided by [Benjamin J. Rosenblatt](https://www.benjrosenblatt.com). The Times obtained results for Rockland County from the [county results page](https://app.enhancedvoting.com/results/public/rockland-county-ny/elections/GE2024Results).
- Precinct results and boundaries for Wisconsin provided by [John D. Johnson](https://johndjohnson.info).
- [Josh Metcalf](https://x.com/josh_metcalf) provided assistance in matching boundaries for Nevada, New Hampshire, Mississippi, Vermont, and Wyoming.
- Township election results for Connecticut, Maine, New Hampshire, Rhode Island, and Vermont provided by the Associated Press.
