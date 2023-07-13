## FHIR Version

Implementations should use FHIR release R4B, or if they've started implementing using R4 already, continue to do so.

The latest release is R4B which adds a small number of changes that are backwards compatible with R4. In most cases, where R4 implementation has already begun this will have no impact as R4B is generally backwards compatible with R4. The difference between the two versions will likely only be relevant if you are implementing an area of the specification that R4B has updated since R4. For full details see: <http://hl7.org/fhir/r4b-explanation.html>


## Produce and publish an Implementation Guide

An implementation guide (IG) should be created for your FHIR implementation.

The IG contains the documentation and machine readable contract that the FHIR API creator and consumers both work from. It defines your API's data model, the terminology (coded values), search functionality, API interactions, and other FHIR conformance artefacts and also all of the human readable information a developer needs to interpret and understand the data communicated via the API.

The Implementation Guide is of key importance as it contains the 'contract' that defines the API implementation and the definition of the data exchanged. It provides significantly richer information than just an OpenAPI spec (both computable and human readable parts) that is often used for an API contract. In addition to supporting technical interoperability, the IG is intended provide sufficient information about the *meaning* of the data the implementation exchanges and has a focus is on including information and functionality that enhances interoperability.

The implementation guide should be created using either the HL7 IG Publisher (and SUSHI toolchain) or [Simplifier.net](http://simplifier.net/ "http://Simplifier.net") tooling as part of the profiling process and implementation definition. This should be the single the source truth for information about the implementation.

The required content of a Health NZ Implementation Guide is standardised here:


## Provide valid conformance artefacts 

Your implementation guide should contain accurate (describing the actual implementation) and valid conformance resources for your implementation (e.g. profile StructureDefinitions, extensions, terminology, capability statements etc.). You should validate these as part of the profiling and implementation guide creation process and these resources are then what you should then be validating resources the implementation produces against.

As these form the computable part of the 'contract' to develop against, these should be rigorously reviewed as part of the profiling process to ensure they are accurate and describe the intended functionality and content.

## Validate your implementation

Ensure your implementation produces and consumes valid FHIR resources. There are a number of tools to support validation of resources. The official FHIR java validator is the best tool to use for offline (non-runtime) validation. The HL7 IG Publisher tool incorporates it in the publishing process if you're using it.

Servers should ensure the resources they return are valid during development and testing (but runtime validation for outgoing resource is not likely to be needed).

For servers accepting updates and creates, runtime validation of incoming resources is highly recommended. You should leverage a FHIR framework for this validation, as there is significant complexity involved in trying to do this from scratch.

For more info on validation see: [Validation - FHIR](https://www.hl7.org/fhir/validation.html)


## Publish a capability statement


Your implementation should publish a CapabilityStatement at the `[baseURL]/meta` endpoint.

The CapabilityStatement should provide an accurate description of the API including supported profiles and interactions. This CapabilityStatement should be identical to that published in the Implementation guide.


## Adopt existing Te Whatu Ora & NZ FHIR context conventions

When profiling, you should aim to adopt any relevent FHIR artefacts, terminology, and standards already developed for the NZ context to ensure your implementation is as interoperable as possible.

### Extensions

The current release of the the [NZ Base Implementation Guide](https://fhir.org.nz/ig/base/ "https://fhir.org.nz/ig/base/") contains common extensions that many NZ FHIR Implementations will need, along with any terminology artifacts referenced by them (ie coded elements)

### Base profiles

The second (and subsequent releases) of NZ Base aim to add to the extensions already published and additionally include a number of NZ Base profiles that should be your starting point for profiling relevant resources.

### Terminology 

Terminology in the above two have also been created, and you should re-use these wherever relevent. Also see the [New Zealand Health Terminology Service](terminology.html)

### Identifiers

The [FHIR Identifier Data type](https://www.hl7.org/fhir/datatypes.html#Identifier "https://www.hl7.org/fhir/datatypes.html#Identifier") is used for any kind of business identifier such as the NHI, CPN, Medical council number, facility ID etc. FHIR uses the 'system' element to allow you to stipulate what namespace each identifier belongs to.

The list of identifier namespace URIs for use in NZ is [available here](https://fhir.org.nz/ig/base/namingSystems.html).

After checking if NZ extensions or artefacts exist, you should also check [HL7 international extensions](https://fhir.org.nz/ig/base/namingSystems.html) and the FHIR Registry to ensure you're re-using rather than developing extensions from scratch.


## Naming standards for defining Canonical URLs

In FHIR, creation of [Canonical Resources](https://build.fhir.org/canonicalresource.html "https://build.fhir.org/canonicalresource.html") are an important part of the profiling process - 'canonical' in this context meaning roughly that the resources describe the definition or rules for how to use something. For example, an instance of a ValueSet resource describes the codes and their descriptions that are valid for use in a particular context, and an instance of a StructureDefinition resource is used to describe the data and structure of an extension.

Canonical resources have a URL element that acts as a globally unique identifier for the instances of these definitional resources across all contexts. These URLs need to be created and (ideally) remain unchanged for the life of the systems using them. The following naming patterns define how canonical URLs should be constructed for Health NZ canonical resources.

**All URLs should start with https**. The rationale for this is if we make any of these resolvable - e.g. via a terminology server, conformance server for profiles and extensions, surfacing NamingSystem resources for identifiers - the systems surfacing these will likely need to meet standard security requirements which may preclude HTTP.

### CodeSystem URLs

-   Codesystem URLs should be created using a base domain of: `standards.digital.health.nz`

-   Under this the URL path `.../ns/[CodeSystem Name]` is used to indicate a namespace for a CodeSystem.

-   The name of the CodeSystem should end in `-code`

The full URL should follow the pattern:

`https://standards.digital.health.nz/ns/[CodeSystem Name]-code`

Examples:

-   <https://standards.digital.health.nz/ns/ethnic-group-level-4-code>

-   <https://standards.digital.health.nz/ns/dhb-code>

### ValueSet URLs

-   ValueSet URLs should be created using a base domain of `nzhts.digital.health.nz` 'nzhts' is chosen as the subdomain for the NZ Health Terminology Service.

-   Under this the URL path of `.../fhir/ValueSet/[ValueSet Name]` is used to indicate a ValueSet.

-   -code at the end of the name is not required as it's clear that it's a set of codes/valueset from the URL. However can be present to make relationship with CodeSystem clear if desired.

The full URL should follow the pattern:

`https://nzhts.digital.health.nz/fhir/ValueSet/[ValueSet Name]`

Examples:

-   <https://nzhts.digital.health.nz/fhir/ValueSet/hpi-contact-type>

-   <https://nzhts.digital.health.nz/fhir/ValueSet/dod-information-source-code>

### StructureDefinition URLs (Profiles, Extensions, Datatypes etc)

-   URLs for artefacts using StructureDefinition should be created using a base domain of: `standards.digital.health.nz`

-   Under this the URL path of `.../fhir/StructureDefinition/name` should be used

The full URL should follow the pattern:

`https://standards.digital.health.nz/fhir/StructureDefinition/[name]`

Examples:

-   <https://standards.digital.health.nz/fhir/StructureDefinition/HNZCovidObservation>

Where an extension or other StructureDefinition has potential for re-use across other contexts, consider taking it to the HL&NZ FHIR Implementation work group. If suitable an HL7NZ URL should be used for the canonical URL of the artefact using the `http://hl7.org.nz/fhir/StructureDefinition/` base address.

