# Units of measurement
Quantitative data is always expressed using a scale, commonly referred to as the unit of measurement (UOM). The representation of any quantity can be conceived as a ‘tuple’ comprised of:
- a number
- the scale or UOM
- (optionally) a quantity-kind, which provides context and semantics for the quantity

The quantity-kind is sometimes required because the same unit of measure can be used for incompatible quantity-kinds (e.g., energy and torque can both be quantified as Newton-metres). When encoding quantities, there are three common patterns:
- Micro-format, using a standard uom notation, and separator between the number and UOM:

<p style="text-align: center;">109.5 km/hr</p>

- Tuple or data-structure:
```
	{
	"@value" : "109.5" ,
	"@type" : "https://si-digital-framework.org/SI/units/kilometre.hour-1"
	}
```
- Fix the UOM for an array or collection, where a header or metadata gives the uom for all values, and the datastream then provides an array of values only:

<p style="text-align: center;">( "km/hr" , ( 60.4 , 75.1, 99.0, 109.5 ) )</p>

For any of these approaches the code or symbol that denotes the uom must come from a well defined system with unambiguous semantics. Note that CDIF Data Description provides properties on variables for specifying both UOM and quantity-kind (see [TBD]()), using the micro-format described in [TBD]()).

## UOM code systems
The following list is provided for reference, giving information about some of the common coding systems used for UOM.

**SI Digital Framework** - https://si-digital-framework.org/
- Scope: Semantic reference for SI base and derived units
- Maintained by BIPM https://www.bipm.org/en/ - the official custodian of the International System of Units (SI) https://en.wikipedia.org/wiki/International_System_of_Units
- Symbols for units follow ISO/IEC 80000 (incl special characters, superscripts etc)
  - m, eV, h, km/h
- Each UOM is denoted by a URI e.g.
  - SI Units and Quantities
    - Unit https://si-digital-framework.org/SI/units/metre
- Quantity https://si-digital-framework.org/quantities/LENG
    - Unit https://si-digital-framework.org/SI/units/pascal
- Quantity https://si-digital-framework.org/quantities/PRES
  - Non-SI units accepted for use with the SI units
    - Unit https://si-digital-framework.org/SI/units/electronvolt
- Quantity https://si-digital-framework.org/quantities/ENGY
    - Unit https://si-digital-framework.org/SI/units/hour
- Quantity https://si-digital-framework.org/quantities/TIME
  - Prefixes
    - Prefix https://si-digital-framework.org/SI/prefixes/kilo
    - Prefix https://si-digital-framework.org/SI/prefixes/nano
  - URIs for compound units based on SI Units are formulated as follows:
    - Unit https://si-digital-framework.org/SI/units/kilometre.hour-1
	
**QUDT** (Quantities, Units, Dimensions and Types) - qudt.org
- Scope: semantic descriptions of UOM for science and engineering
- Enumeration of ca. 2500 UOM - qudt.org/doc/DOC_VOCAB-UNITS.html
- Each UOM is denoted by a URI - e.g.
  - http://qudt.org/vocab/unit/M
    - http://qudt.org/vocab/quantitykind/Distance
    - http://qudt.org/vocab/quantitykind/Length
  - http://qudt.org/vocab/unit/PA
    - http://qudt.org/vocab/quantitykind/Pressure
    - http://qudt.org/vocab/quantitykind/Stress
  - http://qudt.org/vocab/unit/EV
    - http://qudt.org/vocab/quantitykind/Energy
  - http://qudt.org/vocab/unit/HR
    - http://qudt.org/vocab/quantitykind/Time
	- http://qudt.org/vocab/prefix/Kilo
  - http://qudt.org/vocab/prefix/Nano
  - http://qudt.org/vocab/unit/KiloM-PER-HR
- Local-name defined by a rule (non-opaque, almost computable)
- Dereferencing URI gets a definition of the UOM, with link to dimensionality
  - Enough information to support unit conversion
  - Codes and symbols from various systems, including UCUM, SI, and [UN/ECE Recommendation 20](https://unece.org/trade/documents/2021/06/uncefact-rec20-0)
- System also has an enumeration of ‘quantity-kinds’ - i.e. semantics
  - E.g., energy and torque have the same scale and dimensionality, but should not be transformed
- Maintained by volunteers
- Change requests processed ca. weekly - https://github.com/qudt/qudt-public-repo/issues.


**UCUM** (Unified Code for Units of Measure) - https://ucum.org/
- Scope: codes for UOM for science
- Terminal symbols plus a rule for constructing arbitrary units - extensible/scalable
- Codes match traditional scientific notation, with simplifications to match 7-bit ascii keyboard (no special characters, italics, superscripts) - e.g. km/hr, km.hr-1, N.m-2
- Maintained by [LOINC](https://loinc.org/) - Biomedical/clinical standards authority
- Change requests processed slowly - https://github.com/ucum-org/ucum/issues
- Public specification document at https://ucum.org/ucum
- Validation and conversion
  - interactive https://ucum.nlm.nih.gov/ucum-lhc/demo.html
  - API https://ucum.nlm.nih.gov/ucum-service.html.