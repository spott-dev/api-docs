![Spott logo](https://spott-assets.s3.amazonaws.com/marketing/banner-720px.png)

**Welcome to Spott.** We build tools to make easier for developers to work with geographical places, specifically: **cities**, **countries** and **administrative divisions** (states, regions, provinces, etc). At first, we created Spott to solve our own needs in other projects, then we realized other devs had the same problems and launched it to the world.

With Spott you'll be able to:
* **Search places** by a full query or autocompletion, across more than 240,000 records from all the world.
* **Filter them** by country, type and location.
* Find places and get their localized names in more than **20 languages**.
* Get the place where an **IP** address is located.

We'd love to hear what are you using Spott for, or any idea to make it better.

## SDKs

Use our SDKs to make your integration easier:

* [NodeJS](https://www.npmjs.com/package/spott-nodejs-sdk)


## Authentication

To access Spott you need to do it through RapidAPI. It's very easy, just:
1. Create a RapidAPI account.
2. Subscribe to Spott. This will generate an API Key.
3. Start making requests. Send your API Key in `X-RapidAPI-Key` header.

More details [here](https://docs.rapidapi.com/docs/keys).

## Data structure

#### Places

All places returned by Spott might have the following properties.

| Property | Type | Description |
| ---------- | ------ | ------------- |
|`id`| String | Unique identifier given by Spott. |
|`geonameId`| Integer | Unique identifier given by [GeoNames](https://www.geonames.org). |
|`type`| String | The classification of the place. Possible values are: `CITY`, `ADMIN_DIVISION_1`, `ADMIN_DIVISION_2` and `COUNTRY` |
|`name`| String | Default name of the place (usually in English). This property always has a value. |
|`localizedName`| String | Localized name of the place in the requested language. This property is only present when parameter `language` is specified. It's `null` when translation is not available. |
|`population`| Integer | The approximate population living in the place. |
|`elevation`| Float | The approximate elevation from sea level. Value is expressed in meters. |
|`coordinates`| Object | The geographic coordinates where the place is located. |
|`coordinates.latitude`| Float | Latitude component from the geographic coordinates of the place. |
|`coordinates.longitude`| Float | Longitude component from the geographic coordinates of the place. |
|`timezoneId`| String | [Time zone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) associated to the place. This property is `null` for countries since they may have multiple. |
|`score`| Float | Calculated score used to sort results by relevance. This property is only present in results of endpoints that return a list. |
|`adminDivision2`| Object | A minimal version of the Administrative Division level 2 where the place is located. This property is only present for places of type: `CITY`. The object contain the properties: `id`, `geonameId`, `name` and `localizedName`. |
|`adminDivision1`| Object | A minimal version of the Administrative Division level 1 where the place is located. This property is only present for places of types: `CITY` and `ADMIN_DIVISION_1`. The object contain the properties: `id`, `geonameId`, `name` and `localizedName`. |
|`country`| Object | A minimal version of the Country where the place is located. This property is only present for places of types: `CITY`, `ADMIN_DIVISION_1` and `ADMIN_DIVISION_2`. The object contain the properties: `id`, `geonameId`, `name` and `localizedName`. |

#### Countries

**Additionally to Place properties**, a place of type `COUNTRY` might include the properties:

| Property | Type | Description |
| ---------- | ------ | ------------- |
|`iso2`| String | [ISO 3166-1](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes) Alpha-2 code of the country. |
|`iso3`| String | [ISO 3166-1](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes) Alpha-3 code of the country. |
|`isoNumeric`| String | [ISO 3166-1](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes) Numeric code of the country. |
|`continentId`| String | Id of the continent where the country is located. Valids are `AF` (Africa), `AS` (Asia), `EU` (Europe), `NA` (North America), `OC` (Oceania), `SA` (South America) and `AN` (Antarctica). |
|`domain`| String | [Top-level domain](https://en.wikipedia.org/wiki/List_of_Internet_top-level_domains#Country_code_top-level_domains) of the country. |
|`areaSqKm`| Integer | Total area of the country. Expressed in squared kilometers. |
|`currencyCode`| String | Code of the official currency of the country. |
|`currencyName`| String | Name of the official currency of the country. |
|`postalCodeFormat`| String | Format of the postal codes used in the country. |
|`postalCodeRegex`| String | Regular expression to validate the postal codes used in the country. |
|`phoneCodes`| Array[String] | A list of the international phone codes to call a number in the country. |
|`neighbourCountryIds`| Array[String] | A list of ids of the countries that share border with it (neighbours). |
|`languages`| Array[String] | A list of [languages](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) spoken in the country. |
|`locales`| Array[String] | A list of [locales](https://en.wikipedia.org/wiki/Locale_(computer_software)) (language + region) used in the country. |

## Administrative divisions

All countries have different ways to organize or divide by political entities, and each one may call them differently: States, Provinces, Regions and so on. Since names vary from country to country **we call all of them "Administrative Divisions" or "Admin Divisions"** to make it shorter.

Administrative Divisions might have their own subdivisions and this may be repeated multiple times (some countries have 5 levels of subdivisions). **At Spott you can find only divisions up to level 2**, further levels are irrelevant for most applications.

In our data, to reference the first and broadest division level we call it `adminDivision1` and their subdivisions are called `adminDivision2`.

Having said that, the hierarchy of the places by type is:

`COUNTRY` → `ADMIN_DIVISION_1` → `ADMIN_DIVISION_2`* → `CITY`

⚠️ *Not all countries have admin divisions level 2. 

## ID or Geoname ID?

All places returned by Spott have properties `id` and `geonameId`:

* `id`: Is the unique place identifier given by Spott.
* `geonameId`: Is the unique place identifier given by GeoNames, which is an open geographical database.

Although both ids are great to store and reference places from you app, **we recommend to use `id`**. Because:

1. It's more meaningful. `id` is for places and admin divisions easier to understand and remember. `geonameId` is just a number. For example: for "United States" `id` is "US" and `geonameId` is "6252001".
2. It's fully supported by Spott. For example, filter parameters are specified with `id` rather than `geonameId`.

*For cities, both `id` and `geonameId` are the same.

## Name and localizedName

Places returned by Spott have **ALWAYS** the property `name`. Which is the most standard name for that place and it's usually in English.

Additionally, all endpoints support the parameter `language`, which if specified will return the property `localizedName` with the translation of the place in the requested language.

For example:

`GET /places/FR?language=zh` will return `name: "France"` and `localizedName: "法国"`

⚠️ It's common that we don't have a translation for a place, most of the times it's because the translation would be the same than the value in `name`. In those cases the property `localizedName` will have a `null` value. Therefore **we strongly recommend that you application fallbacks to `name` property**.

## Languages

Languages are [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) codes.

Each place is stored with multiple translations and synonyms, which allows Spott to find places with a wide range of strings, rather than just the official name.

For example you could search for Germany as "Deutschland" (in German), "Federal Republic of Germany" (official name) or "ألمانيا" (in Arabic).


#### Supported languages

Our database contains place names in more 181 languages however, there are **21 languages** with significant support:

| ISO 639-1 | ISO Language name |
| ------------ | ----------------------- |
|`ar`|Arabic|
|`bg`|Bulgarian|
|`de`|German|
|`en`|English|
|`eo`|Esperanto|
|`es`|Spanish|
|`fa`|Persian|
|`fr`|French|
|`id`|Indonesian|
|`it`|Italian|
|`ja`|Japanese|
|`kk`|Kazakh|
|`ko`|Korean|
|`nl`|Dutch|
|`pl`|Polish|
|`pt`|Portuguese|
|`ru`|Russian|
|`sr`|Serbian|
|`uk`|Ukrainian|
|`ur`|Urdu|
|`zh`|Chinese|

## IP accuracy

You can send an IP v4 or v6 to `GET /places/ip/{ip}` endpoint to get the place where the IP is located. Spott will return the most accurate place possible, that means **Spott will try to always return a CITY**.

However, depending on the ISP there's no always enough data to calculate the city. In those cases, Spott will return the COUNTRY. You can notice the difference by checking the property `type` on the response.

## Free plan

Spott offers a Free plan with up to 10,000 requests per month to any endpoint.

Unfortunately RapidAPI will require your credit card details to subscribe to our Free plan; that's something we can't change. However, since Free plan is hard limited to 10,000 request, **be confident neither Spott or RapidAPI will change anything** unless you upgrade to a paid plan.

## Data

Our places data is based on [GeoNames.org](https://www.geonames.org), which is the largest open geographical database.

## Help

If you have any comment, question or you want to ask for a feature, **feel free to contact us through RapidAPI or our chat in [spott.dev](https://spott.dev)**.
