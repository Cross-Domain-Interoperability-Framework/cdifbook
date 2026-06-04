# Validation of CDIF metadata documents

One of the requirements for all CDIF conformant metadata records is the inclusion of conformance declaration that is part of the record. This is implemented in the schema.org JSON-LD implementation with a schema:subjectOf key that has @Type schema:Dataset and schema:additionalType dcat:CatalogRecord. We refer to this as the catalog record part of the metadata-- metadata about the metadata. The catalog record includes a dcterms:conformsTo statement that is a list of object references for the profiles the document conforms to. 
```
"schema:subjectOf": {
"@id": "ex:gom-water-quality-wide-2025/catalog-record",
"@type": ["schema:Dataset"],
"schema:additionalType": ["dcat:CatalogRecord"],
....
"dcterms:conformsTo": [
  {"@id": "https://w3id.org/cdif/core/1.1"},
  {"@id": "https://w3id.org/cdif/discovery/1.1"},
  {"@id": "https://w3id.org/cdif/data_description/1.1"}
]  }
```

The conformance uris are set up to resolve as follows:
- Bare uri (e.g.) "https://w3id.org/cdif/core/1.1" resolves to the implementation guide in the release repository. 
- URI with '/schema' appended resolves to the JSON schema for the profile. This schema only validates for classes and properties defined in the profile.
- URI with '/shacl' resolves to a file with SHACL rules to validate the classes and properties defined in the profile.

Thus, to validate an instance document that declares conformance to more than one profile, each profile validation artefact (schema or shacl) must be retrieved and used to test the instance. The CDIF team has implemented a python code to execute this validation workflow. In the https://github.com/Cross-Domain-Interoperability-Framework/validation github the program is ConformanceValidate.py  

To facilitate simple JSON schema validation for common instance documents that use [core + Discovery](https://github.com/Cross-Domain-Interoperability-Framework/doc-corediscovery/blob/main/CDIFDiscoveryProfileStructuredSchema.json), [core + Discovery + DataDescription](https://github.com/Cross-Domain-Interoperability-Framework/doc-discoverydatadescription/blob/main/CDIFDataDescriptionProfileStructuredSchema.json), or [core + Discovery + DataDescription + DataStructure](https://github.com/Cross-Domain-Interoperability-Framework/doc-discoverydatadescriptionstructure/blob/main/CDIFDiscoveryDataDescriptionStructureProfileStructuredSchema.json), composite schema are linked here.  