# HAFAS VBB-API

This document describes the general structure of all responses of the HAFAS system of the Berlin-Brandenburg public transport service (VBB), according to [http://demo.hafas.de/openapi/vbb-proxy/xsd?rest-1.20.xsd](https://github.com/derhuerst/vbb-hafas-docs/blob/master/vbb-hafas.xsd).

**This API description describes version `1.20` of either the data oder the API.** It can be applied to other HAFAS installations, except all the city- and provider-specific customizations.

I was looking for a detailed description of the VBB API, and out of sheer desperation, it created this document. **Please help me keep it up to date**, since it's a relatively small effort just adjusting this to new API versions.





## location service

This service can be used to find stations/stops, addresses and points of interest (POIs).




### `LocationList` (element)

> The location list contains either named coordinates or stops/stations with name and id as a result of a location request. The data of every list entry can be used for further trip or departureBoard requests.

The `LocationList` element contains one or more [`StopLocation`](#todo) or [`CoordLocation`](#todo) elements. It also inherits [common attributes](#todo).




### `StopLocation` (element)

> The element `StopLocation` specifies a stop/station in a result of a location request.

> Contains […][any number of] [lists of notes](#todo) to be displayed for this location, like attributes or footnotes.

The `StopLocation` element has the following attributes.

| attribute | type | description |
|:----------|:-----|:------------|
| `id` (required) | `xs:string` | This ID can either be used as `originId` or `destId` to perform a trip request or to call a departure or arrival board. |
| `extId` (required) | `xs:string` | This ID defines an alternative ID for this stop location and can not be used to perform further requests. |
| `name` (required) | `xs:string` | Contains the output name of this stop or station |
| `lon` (optional) | `xd:decimal` | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) longitude of the geographical position of the stop/station. |
| `lat` (optional) | `xd:decimal` | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) latitude of the geographical position of the stop/station. |
| `track` (optional) | `xs:string` | Track information, if available. |
| `weight` (optional) | `xs:int` | This value specifies some kind of importance of this stop. The more traffic at this stop the higher the weight. The range is between 0 and 32767. This attribute is only available in the location.allstops response |
| `dist` | `xs:int` | This value specifies the distance to the given coordinate if called by a [nearby search request](#todo). |
| `products` | `xs:int` | This value specifies the [products](#todo) available at this location. |
| `meta` | `xs:boolean` | True, if the stop is a meta stop. *todo: what is a meta stop?* |




### `CoordLocation` (element)

> The element `CoordLocation` specifies a coordinate based location in a result of a location request. It contains an output name, latitude, longitude and a type (address or point of interest). The coordinates and the name can be used as origin or destination parameters to perform a trip request.

> Contains a [list of notes](#todo) to be displayed for this location, like attributes or footnotes.

The `CoordLocation` element has the following attributes.

| attribute | type | description |
|:----------|:-----|:------------|
| `name` (required) | `xs:string` | Contains the output name of the address or point of interest |
| `type` (required) | *`xs:string`-based*; `ADR` / `POI` | The attribute type specifies the type of location. Valid values are `ADR` (address) or `POI` (point of interest). This attribute can be used to do some sort of classification in the user interface. For later trip requests it does not have any meaning. |
| `lon` (optional) | `xd:decimal` | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) longitude of the geographical position of the stop/station. |
| `lat` (optional) | `xd:decimal` | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) latitude of the geographical position of the stop/station. |
| `dist` (optional) | `xs:int` | This value specifies the distance to the given coordinate if called by a nearby search request. |




### `LocationNotes` (element)

> Contains […][any number of] [notes](#todo) to be displayed for this location.



#### `LocationNote` (element)

> Text to be displayed

The `LocationNote` element has the following attributes. **Its (text) content is a `xs:string`.**

| attribute | type | description |
|:----------|:-----|:------------|
| `key` (optional) | `xs:string` | An identifier of this note. The identifier is composed of a two letter combination further identifying the content of the note. |
| `type` (optional) | *`xs:string`-based*; `U` / `A` / `I` / `R` / `H` | *Default: `U`* The type of this note. Unknown: `U`. Attribute: `A`. Infotext: `I`. Realtime: `R`. Hint: `H`. |





## journey service

todo: difference to trip service?
todo: what is the purpose?




### `JourneyDetail` (element)

> The journey details contain a list of stops/stations and notes. They also contain the journeys names and types.

`JourneyDetail` inherits [common attributes](#todo). It contains any number of sequences of the following elements.

- [`Stops`](#todo)
- [`GeometryRef`](#todo) (optional)
- [`Names`](#todo) (optional)
- [`Directions`](#todo) (optional)
- [`Notes`](#todo) (optional) Contains notes to be displayed for this trip like attributes or footnotes.
- [`Messages`](#todo) (optional)
- [`JourneyStatus`](#todo) (optional)
- [`Polyline`](#todo) (optional)
- [`ServiceDays`](#todo) (optional)



#### `Stops` (element)

> The list of [journey stops/stations](#todo).

The `Stops` element **contains *two* or more [`Stop` elements](#todo)**.


##### `Stop` (element)

> The element `Stop` contains the name of the stop/station, the route index, the latitude, the longitude, the departure time and date, the arrival time and date, the track, the realtime departure time and date, the realtime arrival time and date and the realtime track.

> Contains […][any number of] [lists of notes](#todo) to be displayed for this location, like attributes or footnotes.

The `Stop` element has the following attributes.

| attribute | type | description |
|:----------|:-----|:------------|
| `name` (required) | `xs:string` | Contains the name of this stop/station. |
| `id` (required) | `xs:string` | Contains the ID of the stop/station. |
| `extId` (required) | `xs:string` | Contains the external ID of the stop/station. [This ID defines an alternative ID for this stop/station and can not be used to perform further requests.] |
| `routeIdx` (optional) | `xs:int` | Route index of a stop/station. Usually starting from `0` and incrementing by `1`. If the route index value jumps, it is most likely that the journey was rerouted. |
| `lon` (optional) | `xd:decimal` | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) longitude of the geographical position of the stop/station. |
| `lat` (optional) | `xd:decimal` | The [WGS84](https://de.wikipedia.org/wiki/World_Geodetic_System_1984) latitude of the geographical position of the stop/station. |
| `arrPrognosisType` (optional) | [`PrognosisType`](#todo) | Prognosis type of arrival date and time. |
| `depPrognosisType` (optional) | [`PrognosisType`](#todo) | Prognosis type of departure date and time. |
| `cancelled` (optional) | `xs:boolean` | *Default: `false`* Will be `true` if this journey is cancelled. |

**The date and time information** provided by the following attributes **is also available as real time data**. **These real time data attributes** work in the same way but **have `rt` in front** (`depTrack` -> `rtDepTrack`).

| attribute | type | description |
|:----------|:-----|:------------|
| `arrTrack` (optional) | `xs:string` | Arrival track information, if available. |
| `arrTime` (optional) | `xs:string` | Arrival time in format `HH:MM:SS`, if available. |
| `arrDate` (optional) | `xs:string` | Arrival date in format `YYYY-MM-DD`, if available. |
| `arrTz` (optional) | `xs:int` | Arrival time zone information in the format `+H` or `-H`. |
| `depTrack` (optional) | `xs:string` | Departure track information, if available. |
| `depTime` (optional) | `xs:string` | Departure time in format `HH:MM:SS`, if available. |
| `depDate` (optional) | `xs:string` | Departure date in format `YYYY-MM-DD`, if available. |
| `depTz` (optional) | `xs:int` | Departure time zone information in the format `+H` or `-H`. |
| `passingTime` (optional) | `xs:string` | Passing time in format `HH:MM:SS`, if available. *todo: what is this?* |
| `passingDate` (optional) | `xs:string` | Passing date in format `YYYY-MM-DD`, if available. *todo: what is this?* |
| `passingTz` (optional) | `xs:int` | Passing time zone information in the format `+H` or `-H`. *todo: what is this?* |



#### `GeometryRef` (element)

> Reference to polyline of this leg.

The `GeometryRef` element has the following attributes.

| attribute | type | description |
|:----------|:-----|:------------|
| `ref` (required) | *todo: is it`xs:string`?* | Contains a URL to call the ReST interface for geometry of this journey. |



#### `Names` (element)

> The list of [journey names](#todo).

The `Names` element contains any number of [`Name` elements](#todo).


##### `Name` (element)

> Contains [the] name of [the] journey.

The `Name` element contains any number of [`Product` elements](#todo). It has the following attributes.

*todo: why does `Name` contain a list of `Product`s??*

| attribute | type | description |
|:----------|:-----|:------------|
| `name` (required) | `xs:string` | Name to be displayed. |
| `number` (required) | `xs:string` | The train number. *todo: why isn't this an `xs:number`?* |
| `category` (required) | `xs:string` | The train category. |
| `routeIdxFrom` (required) | `xs:int` | Defines the first stop/station where this name is valid. See the [Stops list](#todo) for details of the stop/station. |
| `routeIdxTo` (required) | `xs:int` | Defines the last stop/station where this name is valid. See the [Stops list](#todo) for details of the stop/station. |



#### `Directions` (element)

> The list of [journey directions](#todo).

The `Directions` element contains any number of [`Direction` elements](#todo)**.


##### `Direction` (element)

> Direction information. This is usually the last stop of the journey.

The `Direction` element has the following attributes. **Its (text) content is a `xs:string`.**

| attribute | type | description |
|:----------|:-----|:------------|
| `routeIdxFrom` (required) | `xs:int` | Defines the first stop/station where this type is valid. See the [Stops list](#todo) for details of the stop/station. |
| `routeIdxTo` (required) | `xs:int` | Defines the last stop/station where this type is valid. See the [Stops list](#todo) for details of the stop/station. |



#### `Messages` (element)

> Contains […][messages](#todo) to be displayed for this trip.

The `Messages` element contains any number of [`Message` elements](#todo)**.


##### `Message` (element)

> Text to be displayed

The `Message` element has the following attributes. **Its (text) content is a `xs:string`.**

*todo: what the hell is this?*

| attribute | type | description |
|:----------|:-----|:------------|
| `id` (required) | `xs:string` | *todo: document this* |
| `type` (required) | *`xs:string`-based*; `UNDEF` / `JNY` / `ROU` / `LIN` / `REG` / `CNTY` / `GLOB` / `LOC` / `FOOT` / `HEAD` / `PSGR` / `IFRA` | *todo: document this* |
| `act` (required) | `xs:boolean` | *todo: document this* |
| `pub` (required) | `xs:string` | *todo: document this* |
| `head` (required) | `xs:string` | *todo: document this* |
| `lead` (required) | `xs:string` | *todo: document this* |
| `text` (required) | `xs:string` | *todo: document this* |
| `tckr` (required) | `xs:string` | *todo: document this* |
| `priority` (required) | `xs:int` | *todo: document this* |
| `icon` | `xs:string` | *todo: document this* |
| `routeIdxFrom` (optional) | `xs:int` | First stop/station where this message is valid. See the [Stops list in the JourneyDetail response](#todo) for this leg to get more details about this stop/station. |
| `routeIdxTo` (optional) | `xs:int` | Last stop/station where this message is valid. See the [Stops list in the JourneyDetail response](#todo) for this leg to get more details about this stop/station. |



#### `JourneyStatus` (element)

> Contains the status of the journey.

`JourneyStatus`'s **(text) content** is `xs:string`-based and *must* be **one of the following values**.

| value | description |
|:------|:------------|
| `P` | Planned: A planned journey. This is also the default value. |
| `R` | Replacement: The journey was added as a replacement for a planned journey. |
| `A` | Additional: The journey is an additional journey to the planned journeys. |
| `S` | Special: This is a special journey. The exact definition which journeys are considered special up to the customer. |



#### `ServiceDays` (element)

> *Regular* service days ([…] [on which] the train has a *regular* schedule)[…].

> *Irregular* service days for this trip ([…] [on which] the train has a *different* schedule)[…].

The `ServiceDays` element defines *regular* and *irregular* services days. It has the following attributes.

*todo: understand and document this weird way of writing down service days*

| attribute | type | description |
|:----------|:-----|:------------|
| `planningPeriodBegin` (optional) | `xs:string` | Start of the planning period of this data in format `YYYY-MM-DD`. |
| `planningPeriodEnd` (optional) | `xs:string` | End of the planning period of this data in format `YYYY-MM-DD`. |
| `sDaysR` (optional) | `xs:string` | *Regular* service days ([…] [on which] the train has a *regular* schedule)[…]. |
| `sDaysI` (optional) | `xs:string` | *Irregular* service days for this trip ([…] [on which] the train has a *different* schedule)[…]. |
| `sDaysB` (optional) | `xs:string` | *todo: document this* |
| `routeIdxFrom` (optional) | `xs:int` | First stop/station where this […][service rule] is valid. See the [Stops list in the JourneyDetail response](#todo) for this leg to get more details about this stop/station. |
| `routeIdxTo` (optional) | `xs:int` | Last stop/station where this […][service rule] is valid. See the [Stops list in the JourneyDetail response](#todo) for this leg to get more details about this stop/station. |





## common elements




### `Error` (element)

> This element represents the response in case of any error

`Error` inherits [common attributes](#todo).





## common attributes


| attribute | type | description |
|:----------|:-----|:------------|
| `serverVersion` (optional) | `xs:string` | The version of the HAFAS proxy server which was used to calculate that result. |
| `dialectVersion` (optional) | `xs:string` | The version of the response data structure. |
| `version` (optional) | `xs:string` | The data version in the HAFAS server which was used to calculate that result. |
| `errorCode` (optional) | `xs:string` | If the request fails, then the errorCode is filled. |
| `errorText` (optional) | `xs:string` | If the request fails, then the errorText is filled. |
