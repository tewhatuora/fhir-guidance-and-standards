# Implementation standards


9\. Naming standards for defining Canonical URLs
------------------------------------------------

In FHIR, creation of [Canonical Resources](https://build.fhir.org/canonicalresource.html "https://build.fhir.org/canonicalresource.html") are an important part of the profiling process - 'canonical' in this context meaning roughly that the resources describe the definition or rules for how to use something. For example, an instance of a ValueSet resource describes the codes and their descriptions that are valid for use in a particular context, and an instance of a StructureDefinition resource is used to describe the data and structure of an extension.

Canonical resources have a URL element that acts as a globally unique identifier for the instances of these definitional resources across all contexts. These URLs need to be created and (ideally) remain unchanged for the life of the systems using them. The following naming patterns define how canonical URLs should be constructed for Health NZ canonical resources.

**All URLs should start with https**. The rationale for this is if we make any of these resolvable - e.g. via a terminology server, conformance server for profiles and extensions, surfacing NamingSystem resources for identifiers - the systems surfacing these will likely need to meet standard security requirements which may preclude HTTP.

**CodeSystem URLs**

-   Codesystem URLs should be created using a base domain of: `standards.digital.health.nz`

-   Under this the URL path `.../ns/[CodeSystem Name]` is used to indicate a namespace for a CodeSystem.

-   The name of the CodeSystem should end in `-code`

The full URL should follow the pattern:

`https://standards.digital.health.nz/ns/[CodeSystem Name]-code`

Examples:

-   <https://standards.digital.health.nz/ns/ethnic-group-level-4-code>

-   <https://standards.digital.health.nz/ns/dhb-code>

**ValueSet URLs**

-   ValueSet URLs should be created using a base domain of `nzhts.digital.health.nz` 'nzhts' is chosen as the subdomain for the NZ Health Terminology Service.

-   Under this the URL path of `.../fhir/ValueSet/[ValueSet Name]` is used to indicate a ValueSet.

-   -code at the end of the name is not required as it's clear that it's a set of codes/valueset from the URL. However can be present to make relationship with CodeSystem clear if desired.

The full URL should follow the pattern:

`https://nzhts.digital.health.nz/fhir/ValueSet/[ValueSet Name]`

Examples:

-   <https://nzhts.digital.health.nz/fhir/ValueSet/hpi-contact-type>

-   <https://nzhts.digital.health.nz/fhir/ValueSet/dod-information-source-code>

**StructureDefinition URLs (Profiles, Extensions, Datatypes etc)**

-   URLs for artefacts using StructureDefinition should be created using a base domain of: `standards.digital.health.nz`

-   Under this the URL path of `.../fhir/StructureDefinition/name` should be used

The full URL should follow the pattern:

`https://standards.digital.health.nz/fhir/StructureDefinition/[name]`

Examples:

-   <https://standards.digital.health.nz/fhir/StructureDefinition/HNZCovidObservation>

Where an extension or other StructureDefinition has potential for re-use across other contexts, consider taking it to the HL&NZ FHIR Implementation work group. If suitable an HL7NZ URL should be used for the canonical URL of the artefact using the `http://hl7.org.nz/fhir/StructureDefinition/` base address.

