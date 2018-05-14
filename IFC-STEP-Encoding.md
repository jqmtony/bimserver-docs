IFC Encoding

> Updated for version 1.2

# Introduction

As of version 1.2, all types of STEP's String encoding are supported by the IfcStepSerializer and IfcStepDeserializer.

Also see [http://support.bimserver.org/bimserver/topics/make_bimserver_harmonize_string_encodings_in_a_bim_project#reply_8132322]

Further development of the serializers/deserializers encoding/decoding depends on the availability of test files, so if you have problems with certain files, please contact us.

# Differences
For certain characters, multiple different ways of encoding are possible in the STEP standard. BIMserver internally stores all string as UTF-8. BIMserver uses:
  * For 32-126: ISO 8859-1
  * For <255: ISO 10646 / ISO 8859-1 (the same for <255)
  * The rest: UCS-2 or UCS-4, depending on the character

So in certain cases IFC strings from data that comes out of BIMserver will be in different encoding than the strings during checkin. Semantically they hold the same information and the same characters should show up in the viewing application.

Example: In one of the popular IFC test files ("AC11-Institute-Var-2-IFC.ifc") there is a door with the name encoded as "T\S\|r-012", the decoded version is "Tür-012". When this model is serialized as IFC again, the encoded version is "T\X\FCr-012", which is also a valid encoding.