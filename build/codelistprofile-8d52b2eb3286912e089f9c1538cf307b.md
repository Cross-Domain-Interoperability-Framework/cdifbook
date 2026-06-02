# Codelist

A CDIF codelist is implemented as a simple skos:ConceptScheme. The base concept scheme must be documented with the required CDIF Core properties

## Codelist concept scheme

## Codelist concept

### skos:prefLabel
- **Obligation:** mandatory
- **JSON:** `"skos:prefLabel":{string}`
- **Scope note:** A string that identifies the codelist item for human users..

### skos:notation
- **Obligation:** mandatory
- **JSON:** `"skos:notation":{string}`
- **Scope note:** A string that identifies the codelist item for for use in datasets for computer consumption.
