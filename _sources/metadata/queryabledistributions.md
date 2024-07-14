# Queryable distributions

This section provides details for documenting online data distribution approaches.

## Data Provider conventions

**File download distribution**. In the simplest and still common file-based data access scenario, the dataset distribution information in a metadata record includes a link (URL) that will get a file containing the actual resource content in a particular format. The format and information model for the file content must be specified in the distribution object in the metadata. The HTTP protocol is used to GET a resource. Given the view that an API specifies the functions offered and constrains the content of messages transmitted between a client and the agent, this simple file download is not considered an API.

**Service based distribution**. An API builds on a basic communication protocol (e.g. HTTP) by defining functionality and formatting to enable providing the specific data a user requires. This might involve filtering, subsetting, or various transformations for e.g. schema mapping, aggregating or anonymizing data. The focus here is on Web APIs that provide data using a URL for the endpoint location (the server that implements the data access protocol), with parameters to specify the particular data requested. The query parameters might be appended to this base URL as part of the URL, or provided as a message with the request. Metadata content requirements:

- **Service type** -- specify the kind of service. Ideally this should be a resolvable identifier. Currently there is no widely adopted registry for serviceType identifiers, in large part because services might be defined at different levels of granularity, and classifications might focus on function, data formats, thematic content, security, or other aspects of the service definition. For interoperability, there must be an external arrangement between data providers and consumers on the strings that will be used to specify service types. Cardinality: 1..*.
- **Service description document** -- many service implementations include provision for a document that provides a machine-actionable description of a service instance. Examples include OpenAPI documents, OGC Capabilities documents. Software designed to utilise a particular service type will typically include functionality to parse such a description document and engage with the service endpoint. If such a document is available for the service instance providing the resource distribution, it should be included in the distribution metadata. Cardinality (0..1, conditional, required if it exists) 
- **Endpoint URL** -- the base location identifier (URL) through which the service is accessed. If a service description document is provided, the location of the service endpoint would be the other piece of information needed; the endpoint location might be related to the location for the service description document and thus not required to be specified separately. Cardinality (0..1).
- **Access constraints** -- Description of access privileges required to use the API, e.g. registration, licensing, payments. Note that access constraints applying to any distribution of the resource should be specified in the access constraints for the resource description as a whole.

If a service description document is not available, some basic information about the API should be provided in the metadata. The operations offered by the service and the output formats (serialisation scheme and information model) are typically defined in the service specification, and would thus inhere in the service type identifier for clients that recognise the service type. These might be optional in the service type specification, with choices for what it offered specific to a particular endpoint, in which case they should be asserted in the metadata for the particular endpoint.

- **Operations** - specify the functions offered by the service endpoint
- **Output formats** - specify the output formats (serialisation scheme and information model) for
service response documents.
- **URL template** - A template for an HTTP service request that indicates how to invoke the service. The
URL template must follow the conventions specified in [IETF RFC6570](https://datatracker.ietf.org/doc/html/rfc6570). Parameters that are specified
by the user when invoking the service are enclosed in curly brackets ('{ }'). Note that the URL
template must be consistent with the specified service type.
- **Parameters** - some description of the URL parameters that can be specified in the service request
URL. This should include a description of what the parameter does, what data type is used, and the
cardinality for the parameter. If the parameter is populated from a controlled vocabulary, some
specification of the allowed values should be provided, either as an enumerated list or a link to a
vocabulary that will be recognised by users.

## Metadata Provider conventions

Metadata providers offering APIs to search metadata catalogues can be considered a special case because they play a 'middleware' role between resource providers and resource consumers. The only real difference is in the intention of the content offered by the API. The resource they offer is data that is about other data, but the distribution description fits into the above content model. The service type would need to indicate that the API is for discovering information about resources (potentially in some thematic scope). The operations would necessarily include a search operation. The output formats would be the metadata schemes (and optional profiles) offered for service responses, e.g. ISO19115-3 MCP profile, ISO19139 INSPIRE profile, schema.org CDIF profile, DCAT-AP. URL template parameters would include the various properties that are queryable.

## Data User conventions

To identify an API that an application can work with, metadata for the application must specify what formats are acceptable for input data, and the interface(s) used by the application to request input data in that format. The software input file format will be matched with the output formats and the implemented communication protocols will be matched with the service types offered by resource distributions to determine where interoperability is possible.

- **Input file format** -- indicate input formats understood by the application. For interoperability, the format must be specified using strings established by convention between providers and consumers. Use of generic MIME-Types is not sufficient in most cases as these are quite general and can result in data-API matches that don’t actually work.
- **Interface specification**. A communication protocol defines how messages are transmitted between a client and server. The software metadata must specify the application interface(s) (API) that it implements to interact with services providing resources for operation of the software. The API specifies the communication protocol that is used and the formats for requests and expected responses that an application uses to access data from a server.
- **Execution environment**. The software environment needed for operation of the application, e.g. operating system, online/cloud.

For applications operating in a single desktop or local-area network environment, operating systems like Windows, AppleOS, or Linux offer various communication protocols, and applications use various bespoke drivers to implement connection and communication. The simplest case is file-based access using the standard operating system file-open dialogs, and the ‘Interface specification’ is simply ‘local file system’. File-based data retrieval using a URL is similarly simple and ‘Interface specification’ is simply ‘HTTP’. The encodingFormat is the critical information to match data sources with applications in these cases.

The data source might be a relational database like PostgreSQL, MySQL, or any of a variety of noSQL datastores like SOLR, HaDOOP, or MongoDb. The ‘Interface specification’ in this case only needs to indicate that the application has drivers necessary to acquire data from one of these data sources by identifying the data source by name. The encodingFormat used to transmit data between source and consumer is invisible to the user in this case, so the input file format is not required to be specified.

An application might access data via a WebAPI-- using interfaces communicating via the internet and based on HTTP operations (GET, POST, PUT, DELETE), or by tunnelling operation requests embedded in HTTP requests (e.g. OGC GetCapabilities). An application might depend on some particular operations or request parameters (e.g. file formats or profiles), in which case the application metadata ‘Interface Specification’ should be specific to these requirements. Alternatively, the software might operate with any data source that implements a particular interface (API). In this case the software metadata does not need to specify particular file formats or request parameters, these are built into the software and the interface definition. For a particular data access connection, the dataset distribution needs to specify the correct request parameters to get particular data (see [Service based distribution](tbd) ).