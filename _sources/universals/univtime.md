# Time
In order to support discovery and selection, we need to:
1. Describe the time period covered by a dataset (e.g., time-interval, named period, reference period)
2. Say something about the temporal representation within a dataset (e.g., precision, spacing, relative times or sequence/ordering, frequency of time series)
3. Provide information related to the time a dataset was prepared, updated, and its period of validity 

DCAT provides the following predicates that relate a time to the dataset:
1. dcterms:temporal for the temporal extent of the data.
2. dcat:temporalResolution for the temporal precision or spacing of items within a dataset.
3. dcterms:issued, dcterms:modified, dcterms:accrualPeriodicity relate to the dataset management.


The description of time will use some representation within a *temporal reference system* (TRS). The TRS must be known in order to understand the value. There are several representations of time in common use. The Open Geospatial Consortium Abstract Specification - TOPIC 25 - ABSTRACT CONCEPTUAL MODEL FOR TIME [OGC doc 23-049(draft to be published)](https://portal.ogc.org/files/?artifact_id=107087&version=2) provides a conceptual framework for these different representations. 

## Temporal coordinates
Time can be understood as involving positions along the timeline. Position can be expressed as a coordinate in a one-dimensional system. Some practical time systems use this approach explicitly:
- [Unix](https://en.wikipedia.org/wiki/Unix_time) and GPS time are based on seconds counted from an origin in 1970 and 1980, respectively.
	- GPS time is actually represented using a pair of numbers for week number plus seconds into week (optimised for low-bandwidth communication).
- Julian day counts the number of days since the beginning of 4713 BCE.
- Ordinal date counts the number of days from the beginning of a year.
- Geological applications express time in years, or millions of years [Before Present (BP)](https://en.wikipedia.org/wiki/Before_Present), which is defined as 1950 for applications where the precision of the measurement technique warrants.

Note the differing precision of these systems: seconds, days, years, millions of years. The origin of a temporal coordinate system is called the *Epoch*.

## Calendar and clock
Both calendar and clock represent a time position as a set of integer or decimal values for nested elements of progressively finer resolution: year, month, day, hours, minutes, seconds. [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) provides a textual representation or microformat which has been adopted or implemented in most encoding and software systems.

<p style="text-align: center;">Example: "2023-10-04T12:13:30.25+02:00"</p>

This should be understood as a multi-valued tuple providing values for each component of the calendar and clock. Conversion to a coordinate on a time-line can be done arithmetically except that a look-up is required to convert months to their durations in days or seconds, etc. The final two elements of the value, following '+' or '-', indicate the timezone, encoded as the hours and minutes offset from [UTC](https://en.wikipedia.org/wiki/UTC_offset). ISO 8601 uses the Gregorian calendar, whose epoch is the beginning of Year 1 CE.

While other calendars are in contemporary use within some specific communities and for some cultural applications (e.g. Traditional Chinese, Julian, Islamic, Jewish, Baha’i), the Gregorian calendar and the 24-hour clock are universally used for technical purposes.

## Ordered nominal timescales
In historical, archeological, and geological applications, dates may be expressed using a named period, tied to a recognised culture (e.g., ‘bronze age’), dynasty (e.g., ‘Tudor’), or defined by some event(s) observed in the geological record tied to the natural history of a region or the earth (e.g., ‘Proterozoic’). Some of these systems can be mapped to temporal coordinates, though calibrations may be adjusted according to new evidence156. In general, however, only the ordering relationships between members of a nominal timescale are defined - i.e. we know that a nominal date is before, after, or during some other date within the same
system, but not the size of the separation between them.

There are many nominal systems. The [International Chronostratigraphic Chart](https://stratigraphy.org/chart) and its calibration is authoritatively maintained by the [International Commission on Stratigraphy](https://stratigraphy.org/), and provides the temporal reference system used internationally in geology. Most other nominal systems are only used locally, within particular disciplinary communities.

## Recurring and periodic times
Many datasets are structured as time-series, with either regular or irregular sampling period, or frequency of observation. A sequence of time-stamps may be thought of as a one-dimensional grid.

### Usage
Time series - the periodic measurement of the same phenomenon, using the same methods, etc. - are a common way of determining trends or otherwise making observations across time. In some cases, time series are the product of data integration from disparate sources, while in other cases the data are collected as part of time series as part of their design. In the Statistical Data and Metadata Exchange (SDMX) [*Content-Oriented Guidelines*](https://sdmx.org/?page_id=3215) there is a standard enumeration of commonly used frequencies for national and supra-national official statistics. [ISO 8601-1:2019](https://www.iso.org/standard/70907.html#:~:text=This%20document%20specifies%20representations%20of,Coordinated%20Universal%20Time%20(UTC)) adds notation for recurring arbitrary intervals (e.g., 9-5, every Mon-Fri, etc.).