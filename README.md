# Status

will work with same FHIR versions, but will throw errors if you try to profile across R4 versions.

If you build for R4:

- you can include the lesser profiles (see fshFood.fsh) based on MHD. In this case you will get some "Info" and "Warning" in the build regarding various valuesets.
  - Consent.status (l23/c21)	warning	ValueSet http://hl7.org/fhir/ValueSet/consent-state-codes|4.3.0 not found
  - Consent.provision.type (l58/c21)	warning	ValueSet http://hl7.org/fhir/ValueSet/consent-provision-type|4.3.0 not found
  - DocumentReference.status (l19/c22)	information	Reference to draft item http://hl7.org/fhir/document-reference-status|4.0.1
- You can NOT include profiling of R4B PCF, as it will throw errors. Many related to the same valuesets.
- note that the Dependency Table (fragment) does throw a warning, as the html fragment references R4B.
  - output\index​.html​#​/html​/body​/div​/div​/div​/div​/div​/div​/table​/tr​/td​/a at Line 237, column 2592	error	The link 'http://hl7.org/fhir/R4B' for "R4B" cannot be resolved

If you build for R4B:

- you can include the lesser profiles (see fshFood.fsh) based on PCF.
- you can create R4 MHD instances. Although there will be "Info" and "Warning"
  - DocumentReference.status (l19/c22)	information	Reference to draft item http://hl7.org/fhir/document-reference-status|4.3.0
  - DocumentReference​.content[0]​.attachment​.contentType (l23​/c36)	warning	ValueSet http://hl7.org/fhir/ValueSet/mimetypes|4.0.1 not found
- You can NOT include profiling of R4 MHD, as it will throw errors. Many related to the same valuesets

# testmultirelease


Testing if one can have an IG that depends on other IGs coming from R4 and R4B; and profile those further.

ci-build - http://build.fhir.org/ig/JohnMoehrke/testmultirelease/branches/main/index.html

.
