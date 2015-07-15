# HAFAS VBB-API

This document describes the general structure of all responses of the HAFAS system of the Berlin-Brandenburg public transport service (VBB), according to [http://demo.hafas.de/openapi/vbb-proxy/xsd?rest-1.20.xsd](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd).

**This API description describes version `1.20` of either the data oder the API.** It can be applied to other HAFAS installations, except all the city- and provider-specific customizations.

I was looking for a detailed description of the VBB API, and out of sheer desperation, it created this document. **Please help me keep it up to date**, since it's a relatively small effort just adjusting this to new API versions.

- [location service](#todo)
- [journey service](#todo)
- [arrival & departure service](#todo)
- [other stuff](#todo)
- [common attributes](#todo)
- [critique](#todo)





## location service

This service can be used to find stations/stops, addresses and points of interest (POIs).




### [`LocationList`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L45)

> The location list contains either named coordinates or stops/stations with name and id as a result of a location request. The data of every list entry can be used for further trip or departureBoard requests.

The `LocationList` element contains one or more [`StopLocation`](#todo) or [`CoordLocation`](#todo) elements. It also inherits [common attributes](#todo).



#### [`StopLocation`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L57)

> The element `StopLocation` specifies a stop/station in a result of a location request.

> Contains […][any number of] [lists of notes](#todo) to be displayed for this location, like attributes or footnotes.

The `StopLocation` element has the following attributes.

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `id` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | This ID can either be used as `originId` or `destId` to perform a trip request or to call a departure or arrival board. |
| `extId` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | This ID defines an alternative ID for this stop location and can not be used to perform further requests. |
| `name` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Contains the output name of this stop or station |
| `lon` | opt. | [`decimal`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#decimal) | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) longitude of the geographical position of the stop/station. |
| `lat` | opt. | [`decimal`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#decimal) | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) latitude of the geographical position of the stop/station. |
| `track` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Track information, if available. |
| `weight` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | This value specifies some kind of importance of this stop. The more traffic at this stop the higher the weight. The range is between 0 and 32767. This attribute is only available in the location.allstops response |
| `dist` | | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | This value specifies the distance to the given coordinate if called by a [nearby search request](#todo). |
| `products` | | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | This value specifies the [products](#todo) available at this location. |
| `meta` | | [`boolean`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#boolean) | True, if the stop is a meta stop. *todo: what is a meta stop?* |



#### [`CoordLocation`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L125)

> The element `CoordLocation` specifies a coordinate based location in a result of a location request. It contains an output name, latitude, longitude and a type (address or point of interest). The coordinates and the name can be used as origin or destination parameters to perform a trip request.

> Contains a [list of notes](#todo) to be displayed for this location, like attributes or footnotes.

The `CoordLocation` element has the following attributes.

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `name` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Contains the output name of the address or point of interest |
| `type` | req. | *[`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string)-based*; `ADR` / `POI` | The attribute type specifies the type of location. Valid values are `ADR` (address) or `POI` (point of interest). This attribute can be used to do some sort of classification in the user interface. For later trip requests it does not have any meaning. |
| `lon` | opt. | [`decimal`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#decimal) | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) longitude of the geographical position of the stop/station. |
| `lat` | opt. | [`decimal`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#decimal) | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) latitude of the geographical position of the stop/station. |
| `dist` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | This value specifies the distance to the given coordinate if called by a nearby search request. |



#### [`LocationNotes`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L173)

> Contains […][any number of] [notes](#todo) to be displayed for this location.


##### [`LocationNote`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L187)

> [The] Text to be displayed

The `LocationNote` element has the following attributes. **Its (text) content is a [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string).**

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `key` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | An identifier of this note. The identifier is composed of a two letter combination further identifying the content of the note. |
| `type` | opt. | *[`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string)-based*; `U` / `A` / `I` / `R` / `H` | *Default: `U`* The type of this note. Unknown: `U`. Attribute: `A`. Infotext: `I`. Realtime: `R`. Hint: `H`. |





## journey service

*todo: difference to [trip service](#todo)?*
*todo: what is the purpose?*




### [`JourneyDetail`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L796)

> The journey details contain a list of stops/stations and notes. They also contain the journeys names and types.

`JourneyDetail` inherits [common attributes](#todo). It contains any number of sequences of the following elements.

1. [`Stops`](#todo)
2. [`GeometryRef`](#todo) (optional)
3. [`Names`](#todo) (optional)
4. [`Directions`](#todo) (optional)
5. [`Notes`](#todo) (optional) Contains notes to be displayed for this trip like attributes or footnotes.
6. [`Messages`](#todo) (optional)
7. [`JourneyStatus`](#todo) (optional)
8. [`Polyline`](#todo) (optional)
9. [`ServiceDays`](#todo) (optional)



#### [`Stops`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L819)

> The list of [journey stops/stations](#todo).

The `Stops` element **contains *two* or more [`Stop` elements](#todo)**.


##### [`Stop`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L825)

> The element `Stop` contains the name of the stop/station, the route index, the latitude, the longitude, the departure time and date, the arrival time and date, the track, the realtime departure time and date, the realtime arrival time and date and the realtime track.

> Contains […][any number of] [lists of notes](#todo) to be displayed for this location, like attributes or footnotes.

The `Stop` element has the following attributes.

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `name` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Contains the name of this stop/station. |
| `id` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Contains the ID of the stop/station. |
| `extId` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Contains the external ID of the stop/station. [This ID defines an alternative ID for this stop/station and can not be used to perform further requests.] |
| `routeIdx` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Route index of a stop/station. Usually starting from `0` and incrementing by `1`. If the route index value jumps, it is most likely that the journey was rerouted. |
| `lon` | opt. | [`decimal`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#decimal) | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) longitude of the geographical position of the stop/station. |
| `lat` | opt. | [`decimal`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#decimal) | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) latitude of the geographical position of the stop/station. |
| `arrPrognosisType` | opt. | [`PrognosisType`](#todo) | Prognosis type of arrival date and time. |
| `depPrognosisType` | opt. | [`PrognosisType`](#todo) | Prognosis type of departure date and time. |
| `cancelled` | opt. | [`boolean`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#boolean) | *Default: `false`* Will be `true` if this journey is cancelled. |

**The date, time and track information** provided by the following attributes **is also available as real time data**. **These real time data attributes** work in the same way but **have `rt` in front** (`depTrack` -> `rtDepTrack`).

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `arrTrack` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Arrival track information, if available. |
| `arrTime` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Arrival time in format `HH:MM:SS`, if available. |
| `arrDate` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Arrival date in format `YYYY-MM-DD`, if available. |
| `arrTz` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Arrival time zone information in the format […][`+H` or `-H`]. |
| `depTrack` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Departure track information, if available. |
| `depTime` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Departure time in format `HH:MM:SS`, if available. |
| `depDate` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Departure date in format `YYYY-MM-DD`, if available. |
| `depTz` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Departure time zone information in the format […][`+H` or `-H`]. |
| `passingTime` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Passing time in format `HH:MM:SS`, if available. *todo: what is this?* |
| `passingDate` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Passing date in format `YYYY-MM-DD`, if available. *todo: what is this?* |
| `passingTz` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Passing time zone information in the format […][`+H` or `-H`]. *todo: what is this?* |



#### [`GeometryRef`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1094)

> Reference to polyline of this leg.

The `GeometryRef` element has the following attributes.

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `ref` | req. | *todo: is it[`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string)?* | Contains a URL to call the ReST interface for geometry of this journey. |



#### [`Names`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1106)

> The list of [journey names](#todo).

The `Names` element contains any number of [`Name` elements](#todo).


##### [`Name`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1112)

> Contains [the] name of [the] journey.

The `Name` element contains any number of [`Product` elements](#todo). It has the following attributes.

*todo: why does `Name` contain a list of `Product`s??*

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `name` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Name to be displayed. |
| `number` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | The train number. *todo: why isn't this an `number`?* |
| `category` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | The train category. |
| `routeIdxFrom` | req. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Defines the first stop/station where this name is valid. See the [Stops list](#todo) for details of the stop/station. |
| `routeIdxTo` | req. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Defines the last stop/station where this name is valid. See the [Stops list](#todo) for details of the stop/station. |



#### [`Directions`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1000)

> The list of [journey directions](#todo).

The `Directions` element contains any number of [`Direction` elements](#todo)**.


##### [`Direction`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1006)

> Direction information. This is usually the last stop of the journey.

The `Direction` element has the following attributes. **Its (text) content is a [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string).**

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `routeIdxFrom` | req. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Defines the first stop/station where this type is valid. See the [Stops list](#todo) for details of the stop/station. |
| `routeIdxTo` | req. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Defines the last stop/station where this type is valid. See the [Stops list](#todo) for details of the stop/station. |



#### [`Messages`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1034)

> Contains […][messages](#todo) to be displayed for this trip.

The `Messages` element contains any number of [`Message` elements](#todo)**.


##### [`Message`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1040)

> [The] Text to be displayed

The `Message` element has the following attributes. **Its (text) content is a [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string).**


*todo: what the hell is this?*

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `id` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *@VBB @HaCon what is this??* |
| `type` | req. | *[`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string)-based*; `UNDEF` / `JNY` / `ROU` / `LIN` / `REG` / `CNTY` / `GLOB` / `LOC` / `FOOT` / `HEAD` / `PSGR` / `IFRA` | *@VBB @HaCon what is this??* |
| `act` | req. | [`boolean`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#boolean) | *@VBB @HaCon what is this??* |
| `pub` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *@VBB @HaCon what is this??* |
| `head` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *@VBB @HaCon what is this??* |
| `lead` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *@VBB @HaCon what is this??* |
| `text` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *@VBB @HaCon what is this??* |
| `tckr` | req. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *@VBB @HaCon what is this??* |
| `priority` | req. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | The priority of this message. A lower priority value means a higher importance. A priority with value `-1` means priority is undefined. |
| `icon` | | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *@VBB @HaCon what is this??* |
| `routeIdxFrom` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | First stop/station where this message is valid. See the [`Stops` list in the JourneyDetail response](#todo) for this leg to get more details about this stop/station. |
| `routeIdxTo` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Last stop/station where this message is valid. See the [`Stops` list in the JourneyDetail response](#todo) for this leg to get more details about this stop/station. |



#### [`JourneyStatus`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1153)

> Contains the status of the journey.

`JourneyStatus`'s **(text) content** is [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string)-based and *must* be **one of the following values**.

| value | description |
|:------|:------------|
| `P` | Planned: A planned journey. This is also the default value. |
| `R` | Replacement: The journey was added as a replacement for a planned journey. |
| `A` | Additional: The journey is an additional journey to the planned journeys. |
| `S` | Special: This is a special journey. The exact definition which journeys are considered special up to the customer. |



#### [`ServiceDays`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L1182)

> *Regular* service days ([…] [on which] the train has a *regular* schedule)[…].

> *Irregular* service days for this trip ([…] [on which] the train has a *different* schedule)[…].

The `ServiceDays` element defines *regular* and *irregular* services days. It has the following attributes.

*todo: understand and document this weird way of writing down service days*

| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `planningPeriodBegin` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | Start of the planning period of this data in format `YYYY-MM-DD`. |
| `planningPeriodEnd` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | End of the planning period of this data in format `YYYY-MM-DD`. |
| `sDaysR` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *Regular* service days ([…] [on which] the train has a *regular* schedule)[…]. |
| `sDaysI` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *Irregular* service days for this trip ([…] [on which] the train has a *different* schedule)[…]. |
| `sDaysB` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | *@VBB @HaCon what is this??* |
| `routeIdxFrom` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | First stop/station where this […][service rule] is valid. See the [`Stops` list in the JourneyDetail response](#todo) for this leg to get more details about this stop/station. |
| `routeIdxTo` | opt. | [`int`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#integer) | Last stop/station where this […][service rule] is valid. See the [`Stops` list in the JourneyDetail response](#todo) for this leg to get more details about this stop/station. |





## common elements




### [`Error`](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L34)

> This element represents the response in case of any error

`Error` inherits [common attributes](#todo).





## [common attributes](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd#L6)


| attribute | use | type | description |
|:----------|:----|:-----|:------------|
| `serverVersion` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | The version of the HAFAS proxy server which was used to calculate that result. |
| `dialectVersion` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | The version of the response data structure. |
| `version` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | The data version in the HAFAS server which was used to calculate that result. |
| `errorCode` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | If the request fails, then the `errorCode` is filled. |
| `errorText` | opt. | [`string`](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#string) | If the request fails, then the `errorText` is filled. |







## critique

*Disclaimer: My viewpoint might be relatively superficial (since I'm not a co-author of HAFAS), maybe even arrogant. But at least these are the questions and hurdles I have had to fight with, so please consider making this API truly developer-friendly.*

The structures of **`Stop`, `Arrival` and `Departure`** all look very similar. Why not **unify them into `Stops`** with a `role`/`action` attribute (with the values `enterTrain`, `stop`, `changeTrain` and `leaveTrain`)?

**`Arrival` and `Departure`** as well as **`ArrivalBoard` and `DepartureBoard`**, respectively, **could easily be unified**. Again, a single attribute could give enough context.

And finally: **Please please please provide XML *and* JSON *consistently* on *all* API endpoints!**




### rather unimportant stuff

I think the `GeometryRef` and `GisRef` stuff is too loosely related to the purpose of this API. Get rid of it and move to its own API endpoint.
