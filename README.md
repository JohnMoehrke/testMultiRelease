# testmultirelease

Testing if one can have an IG that depends on other IGs coming from R4 and R4B; and profile those further.

ci-build - http://build.fhir.org/ig/JohnMoehrke/testmultirelease/branches/main/index.html

## Status

An IG that wants to further refine a dependency IG, must be built against the same version as that IG.
An IG can dependency an IG from the other (R4 vs R4B) and use it as is, but can not profile it.
If you try to further refine profiles from an IG built on the opposite R4/R4B version, you will get [failures](#current-failure)

## further experimentation

originally I thought it was because of dependency to two (or more) IGs from different versions. But the problem seens to be simply when this IG is being built for one FHIR R4 version, yet the dependency is from the other (R4 vs R4B)

If you build this IG for R4:

- you can include the lesser profiles (see fshFood.fsh) based on MHD. In this case you will get some "Info" and "Warning" in the build regarding various valuesets.
  - Consent.status (l23/c21)	warning	ValueSet http://hl7.org/fhir/ValueSet/consent-state-codes|4.3.0 not found
  - Consent.provision.type (l58/c21)	warning	ValueSet http://hl7.org/fhir/ValueSet/consent-provision-type|4.3.0 not found
- You can NOT include profiling of R4B PCF, as it will throw errors. Many related to the same valuesets.
- note that the Dependency Table (fragment) does throw a warning, as the html fragment references R4B.
  - output\index​.html​#​/html​/body​/div​/div​/div​/div​/div​/div​/table​/tr​/td​/a at Line 237, column 2592	error	The link 'http://hl7.org/fhir/R4B' for "R4B" cannot be resolved

If you build this IG for R4B:

- you can't dependency MHD and profile MHD profiles, throws errors and dies
- you can include the lesser profiles (see fshFood.fsh) based on PCF.
- you can create R4 MHD instances. Although there will be "Info" and "Warning"
  - DocumentReference​.content[0]​.attachment​.contentType (l23​/c36)	warning	ValueSet http://hl7.org/fhir/ValueSet/mimetypes|4.0.1 not found
- You can NOT include profiling of R4 MHD, as it will throw errors. Many related to the same valuesets

If you do not refine profiles from dependency IGs, then no problem.

## Current failure

```
onGenerate:
Template Done                                                                    (01:28.354)
Generate HTML Outputs                                                            (01:28.357)
Produce outputs for ImplementationGuide/johnmoehrke.testmultirelease.example     (01:28.379)
Produce outputs for Consent/ex-consent-basic-treat                               (01:28.699)
Produce outputs for DocumentReference/ex-documentreference                       (01:28.777)
Produce outputs for DocumentReference/ex-DocumentReferenceMinimalLesser          (01:28.868)
Produce outputs for Organization/ex-organization                                 (01:28.949)
Produce outputs for Patient/ex-patient                                           (01:28.992)
Produce outputs for StructureDefinition/testMultiRelease.LesserDocumentReference (01:29.042)
Value set 'http://hl7.org/fhir/ValueSet/c80-doc-typecodes' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.type not found
Value set 'http://hl7.org/fhir/ValueSet/document-classcodes' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.category not found
Value set 'http://hl7.org/fhir/ValueSet/document-relationship-type|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.relatesTo.code not found
Value set 'http://hl7.org/fhir/ValueSet/mimetypes|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.content.attachment.contentType not found
Value set 'http://hl7.org/fhir/ValueSet/c80-doc-typecodes' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.type not found
Value set 'http://hl7.org/fhir/ValueSet/document-classcodes' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.category not found
Value set 'http://hl7.org/fhir/ValueSet/c80-doc-typecodes' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.type not found
Value set 'http://hl7.org/fhir/ValueSet/document-classcodes' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.category not found
Value set 'http://hl7.org/fhir/ValueSet/document-relationship-type|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.relatesTo.code not found
Value set 'http://hl7.org/fhir/ValueSet/mimetypes|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.LesserDocumentReference#DocumentReference.content.attachment.contentType not found
ERROR StatusLogger Log4j2 could not find a logging implementation. Please add log4j-core to the classpath. Using SimpleLogger to log to the console...
Produce outputs for StructureDefinition/testMultiRelease.lesserProfileBundle     (01:36.523)
Value set 'http://hl7.org/fhir/ValueSet/bundle-type|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.type not found
Value set 'http://hl7.org/fhir/ValueSet/search-entry-mode|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry.search.mode not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/search-entry-mode|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:SubmissionSet.search.mode not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:SubmissionSet.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/search-entry-mode|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:DocumentRefs.search.mode not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:DocumentRefs.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/search-entry-mode|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:UpdateDocumentRefs.search.mode not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:UpdateDocumentRefs.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/search-entry-mode|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:Documents.search.mode not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:Documents.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/search-entry-mode|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:FhirDocuments.search.mode not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:FhirDocuments.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/search-entry-mode|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:Folders.search.mode not found
Value set 'http://hl7.org/fhir/ValueSet/search-entry-mode|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:Patient.search.mode not found
Value set 'http://hl7.org/fhir/ValueSet/bundle-type|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.type not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:SubmissionSet.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:DocumentRefs.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:UpdateDocumentRefs.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:Documents.request.method not found
Value set 'http://hl7.org/fhir/ValueSet/http-verb|4.0.1' at http://johnmoehrke.github.io/testmultirelease/StructureDefinition/testMultiRelease.lesserProfileBundle#Bundle.entry:FhirDocuments.request.method not found
Generate Summaries                                                               (01:45.499)
Generating Summary Outputs                                                       (01:45.500)
Sending Usage Stats to Server                                                    (02:08.983)
## Memory (MB): Use = 824, Free = 403, Total = 1228, Max = 4068
Reclaiming memory...
## Memory (MB): Use = 602, Free = 625, Total = 1228, Max = 4068
Run jekyll: jekyll build --destination "C:\Users\johnm\git\testMultiRelease\output" (in folder C:\Users\johnm\git\testMultiRelease\temp\pages) (02:09.416)
Jekyll: Source: C:/Users/johnm/git/testMultiRelease/temp/pages                   (02:12.107)
Jekyll: Generating...                                                            (02:12.109)
Jekyll: done in 6.979 seconds.                                                   (02:19.086)

Installing johnmoehrke.testmultirelease.example#dev done.
Checking Output HTML                                                             (02:23.643)
found 617 files                                                                  (02:23.686)
.........................................
..Publishing Content Failed: Illegal char <|> at index 61: C:\Users\johnm\git\testMultiRelease\output\composition-status|4.0.1.html (02:28.008)
                                                                                 (02:28.010)
Use -? to get command line help                                                  (02:28.010)
                                                                                 (02:28.011)
Stack Dump (for debugging):                                                      (02:28.011)
java.nio.file.InvalidPathException: Illegal char <|> at index 61: C:\Users\johnm\git\testMultiRelease\output\composition-status|4.0.1.html
        at java.base/sun.nio.fs.WindowsPathParser.normalize(WindowsPathParser.java:182)
        at java.base/sun.nio.fs.WindowsPathParser.parse(WindowsPathParser.java:153)
        at java.base/sun.nio.fs.WindowsPathParser.parse(WindowsPathParser.java:77)
        at java.base/sun.nio.fs.WindowsPath.parse(WindowsPath.java:92)
        at java.base/sun.nio.fs.WindowsFileSystem.getPath(WindowsFileSystem.java:232)
        at java.base/java.nio.file.Path.of(Path.java:147)
        at org.hl7.fhir.utilities.PathBuilder.checkPathIsChildOfTarget(PathBuilder.java:143)
        at org.hl7.fhir.utilities.PathBuilder.buildPath(PathBuilder.java:130)
        at org.hl7.fhir.utilities.Utilities.path(Utilities.java:647)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkResolveLink(HTMLInspector.java:703)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:542)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.checkLinks(HTMLInspector.java:557)
        at org.hl7.fhir.igtools.publisher.HTMLInspector.check(HTMLInspector.java:288)
        at org.hl7.fhir.igtools.publisher.Publisher.generate(Publisher.java:7282)
        at org.hl7.fhir.igtools.publisher.Publisher.createIg(Publisher.java:1068)
        at org.hl7.fhir.igtools.publisher.Publisher.execute(Publisher.java:898)
        at org.hl7.fhir.igtools.publisher.Publisher.main(Publisher.java:11775)
Press any key to continue . . .
```

