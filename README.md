# OpenWeatherMap-VirtSrvc
A virtuallized service for [OpenWeatherMap API](http://openweathermap.org/api). Since the data weather data is constantly changing, integration and end-to-end testing of application that use this service can be difficult and the full test code coverage is likely impossible to achieve. The virtualized service, on the other hand, pretends to be and behaves like the real service from the API contract standpoint, but it responds predictably to the test API calls. It also opens additional opportunities important during testing, such as simulating slow response, timeouts, error codes, etc. that are not possible to test against the real service.

## GitHub Projects That Use This Service
+ [OpenWeatherMap-PHP-Api](https://github.com/cmfcmf/OpenWeatherMap-PHP-Api): [PHP](http://php.net/) interface for OWM
+ [openweathermap](https://github.com/briandowns/openweathermap): [Go](https://golang.org/) (golang) interface for OWM
+ [ROpenWeatherMap](https://github.com/mukul13/ROpenWeatherMap) (planning): R interface for OWM
+ [PyOWM](https://github.com/csparpa/pyowm) (planning): Python interface for OWM

If you would like to be the next project to take advantage of this resource on GitHub, just create a new [Issue](https://github.com/aseriy/OpenWeatherMap-VirtSrvc/issues) and express your interest.

## Design
The virtualized service, a.k.a. stub, is implemented using IBM Rational Integration Tester (RIT). The Starter Edition (SE) is available for [FREE](https://developer.ibm.com/testing/resources/category/starter-edition/). The stub handles the API calls and responds based on the test data that is maintained in the external MySQL database.
### API Calls
The following API calls are currently support:
+ [Current Weather](http://openweathermap.org/current)
  - By city name
  - By city ID
  - By geographic coordinates
  - By ZIP code in USA (limited at this time, with the following values available:

| ZIP      | City         |
|----------|--------------|
| 19125,US | Philadelphia |
| 60290,US | Chicago      |
| 02108,US | Boston       |
| 30301,US | Atlanta      |

+ [Daily Forecase](http://openweathermap.org/forecast16) for 1, 2, 3, 5, 7 and 10 days
  - By city name
  - By city ID
  - By geographic coordinates

All API calls can return XML or JSON response for metric and imperial units, as well as the following languages are implemented: bg, de, en, fr, it, nl, pl, pt, ru, se, sp, tr, zh_cn, zh_tw.

Other calls are coming up soon.

### Database
The test data resides in the MySQL database called `owm`. The current DB dump is saved in `db/own.sql`.

Since the database has to store text in foreign languages, make sure that the support of UTF-8 is enabled. If you're running on Red Hat Linux, the MySQL configuration file `db/my.cnf.redhat` is included. On Red Hat it's located at `/etc/my.cnf`. On the various Linux distributions, Windows and Mac, the configuration should be similar. To restore the DB on your system, you need to create a new database, then give some user enough privileges to create and populate tables. You can then execute the above file, see [instructions](https://dev.mysql.com/doc/refman/5.7/en/mysql-batch-commands.html).

## Test Data
You can currently query for weather in the following cities from the table below by the City ID or GEO coordinates.

| City          | Country | ID      | Longitude   | Latitude   |
|---------------|---------|--------:|------------:|-----------:|
| Arkhangelsk   | RU      |  581049 | 40.543301   | 64.5401    |
| Athens        | GR      |  264371 | 23.716221   | 37.97945   |
| Atlanta       | US      | 4180439 | -84.387978  | 33.749001  |
| Beijing       | CN      | 1816670 | 116.397232  | 39.907501  |
| Belem         | BR      | 3405870 | -48.50444   | -1.45583   |
| Berlin        | DE      | 2950159 | 13.41053    | 52.524368  |
| Boston        | US      | 4930956 | -71.059769  | 42.358429  |
| Chicago       | US      | 4887398 | -87.650047  | 41.850029  |
| Clonakilty    | IE      | 2965402 | -8.87056    | 51.623058  |
| Corrigan      | US      | 4683453 | -94.827148  | 30.996861  |
| Dubai         | AE      |  292223 | 55.304722   | 25.258169  |
| Hamburg       | DE      | 2911298 | 10          | 53.549999  |
| Helena        | US      | 5656882 | -112.03611  | 46.592709  |
| Johannesburg  | ZA      |  993800 | 28.043631   | -26.202271 |
| Las Vegas     | US      | 5506956 | -115.137222 | 36.174969  |
| London        | CA      | 6058560 | -81.23304   | 42.983391  |
| London        | GB      | 2643743 | -0.12574    | 51.50853   |
| Madrid        | ES      | 3117735 | -3.70256    | 40.4165    |
| Miami         | US      | 4164138 | -80.193657  | 25.774269  |
| Montreal      | CA      | 6077243 | -73.587807  | 45.508839  |
| Moscow        | RU      |  524901 | 37.615555   | 55.75222   |
| Moscow        | US      | 5202009 | -75.518517  | 41.33675   |
| New York      | US      | 5128581 | -74.005966  | 40.714272  |
| Newark        | US      | 5101798 | -74.172371  | 40.735661  |
| Paris         | FR      | 2988507 | 2.3488      | 48.853409  |
| Philadelphia  | US      | 4560349 | -75.163788  | 39.952339  |
| Prague        | CZ      | 3067696 | 14.42076    | 50.088039  |
| San Diego     | US      | 5391811 | -117.157257 | 32.715328  |
| San Francisco | US      | 5391959 | -122.419418 | 37.774929  |
| Seattle       | US      | 5809844 | -122.332069 | 47.606209  |
| Sydney        | AU      | 2147714 | 151.207321  | -33.867851 |
| Tel Aviv      | IL      |  293397 | 34.780571   | 32.080879  |
| Tokyo         | JP      | 1850147 | 139.691711  | 35.689499  |

For querying by city name, the location is resolved based on the following table:

| Query           | City          | Country |
|-----------------|---------------|---------|
| Athens          | Athens        | GR      |
| Atlanta         | Atlanta       | US      |
| Berlin          | Berlin        | DE      |
| Boston          | Boston        | US      |
| Chicago         | Chicago       | US      |
| Corrigan,US     | Corrigan      | US      |
| Dubai           | Dubai         | AE      |
| Hamburg         | Hamburg       | DE      |
| Helena          | Helena        | US      |
| Helena,US       | Helena        | US      |
| Las Vegas       | Las Vegas     | US      |
| London          | London        | GB      |
| London,CA       | London        | CA      |
| London,UK       | London        | GB      |
| Madrid          | Madrid        | ES      |
| Miami           | Miami         | US      |
| Montreal        | Montreal      | CA      |
| Moscow          | Moscow        | RU      |
| Moscow,US       | Moscow        | US      |
| New York        | New York      | US      |
| Newark          | Newark        | US      |
| Newark,US       | Newark        | US      |
| Paris           | Paris         | FR      |
| Philadelphia    | Philadelphia  | US      |
| Philadelphia,US | Philadelphia  | US      |
| Prague          | Prague        | CZ      |
| San Diego       | San Diego     | US      |
| San Diego, CA   | San Diego     | US      |
| San Francisco   | San Francisco | US      |
| Seattle         | Seattle       | US      |
| Tel Aviv        | Tel Aviv      | IL      |
| Tokyo           | Tokyo         | JP      |

Notice that in order to query by the city name, it **must** be included in `city_search` table:

| Field   | Type       | Null | Key | Default | Extra |
|---------|------------|------|-----|---------|-------|
| query   | char(50)   | NO   | PRI | NULL    |       |
| city_id | bigint(20) | NO   |     | NULL    |       |

where `city_id` is the internat OWM city ID.

### Data Sets
The DB is designed to accomodate multiple data sets. Different data sets may be tailored specifically for test scenarios that you may need in your particular situation. For example, if you're developing a mobile appication, you may want to test if it correctly displays the graphics for the different weather conditions, i.e. snowfall in Siberia, sandstorm in Africa, hurricane in the Midwest, etc., where the current weather and the forecast will be completely different. A separate data set for each situation will solve this problem. Each data set is associated with a dedicated API key, a.k.a. APPID. By using different APPID's, you can access different data sets. Currently, there is only one data set with APPID

	50b2ae8523a458183982bf56254556dc

More data sets will be created in the future to accomodate the projects that use this virtualized service. If you would like a specific data set created for your needs, please create a new [Issue](https://github.com/aseriy/OpenWeatherMap-VirtSrvc/issues) with your test requirements.

## License
Licensed under the Apache License, Version 2.0 (the "License"); you may not use this except in compliance with the License. You may obtain a copy of the License at

     http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

