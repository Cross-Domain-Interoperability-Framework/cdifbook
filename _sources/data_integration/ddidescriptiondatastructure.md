#  DDI-CDI description of Data Structure

A Data Set has a fixed structure. A data set might be the result of querying services, with the Data Structure generated when the query is executed along with the resulting data. There are three types of Data Set: Wide, Long, and Dimensional. The descriptions of these are very similar, differing mainly in the way they identify specific data cells. Variables are described separately from the Data Structure, because variables can be re-used across different structures. For instance, a ‘Nation’ variable represented with a two-character ISO Country Code might be reused across several different data sets, for example SDG Indicators and the Minimum Set of Gender Indicators. 


The role of a variable within a specific Data Structure is represented by the cdi:Component class. There are several specific types of Components: identifiers, measures, attributes, and so on. These are described as appropriate for each of the three types of data structure.

## Wide Data Structures

Most data sets can be described as Wide Data Sets in DDI-CDI – these are those cases where each row in a tabular view of the data is a record, and each column is a single variable. To describe a Wide Data Structure, each variable (here we use Instance Variables, acting in their capacity as Represented Variables) is assigned as defining a specific type of Component. In Wide Data Structures, the only Component classes we need are Identifier Components (cdi:IdentifierComponent), Measure Components (cdi:MeasureComponent), and Attribute Components (cdi:AttributeComponent). The diagram below shows how these and other classes are related.

![Wide data structures](./figures/widedatastructures.jpg)

Figure. Wide data structures.

Measure Components hold the observed or measured values in our data. Identifier Components hold specific identifiers assigned to the cases in our data: each record is about a single ‘case’, and these are sometimes assigned identifiers to distinguish them. As in the example provided below, this could be a tax-payer ID for a person, for example, or might be a randomly assigned number in the case where the cases have been anonymised to protect their identities.

![Wide data example](./figures/widedataexample.jpg)

Figure. Example wide data. TaxpayerID is an identifier component, <br>Taxes Paid is the measure component, <br>and Year is an attribute component.

It is sometimes the case that an identifier Component is sufficient to disambiguate all the records in the data set: Identifier indicating the row/case, and the variable telling you which value in that record. Often, however, there will be additional fields which are needed (like a timestamp, in the case where the same case was measured/observed on more than one occasion). Attribute Components contain additional information about the measurements/observations which are applicable to each case. Typical Attribute Components are time stamps, geographical coordinates, statuses, or other data related to the context of the measurement or observation. The Primary Key is the set of Components needed to uniquely identify each record in the data set. It is not typically the case that the Measure Components form part of the Primary Key.