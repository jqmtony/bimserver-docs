# Introduction

The BIMserver is storing all data in a key-value store. The key-value store is being accessed through the [KeyValueStore Interface](http://tools.bimtoolset.org/BIMserver/nightly%20build%20javadoc/org/bimserver/database/KeyValueStore.html ). Right now there is only one implementation, which is using the open source [BerkeleyDB Java Edition](http://www.oracle.com/technetwork/database/berkeleydb/overview/index-093405.html).

A key value store is defined as:
  * A set of named tables
  * Each table has 2 columns, key and value
  * Both the key and value columns can contain an arbitrarily sized byte array, this size may vary per record
  * All keys in a table are always ordered
  * No duplicate keys can exist

# Principles

  * Models are stored in projects
  * Each new version of the model is stored a new revision
  * Each revision of the project should always be accessible
  * Revisions, once stored, can never be changed
  * Objects in a project can only reference other objects in the same project.

# Details

Each record in a table has the following layout:

| Key | Value |
| --- | --- |
| Pid (4 bytes) + Oid (8 bytes) + Rid (4 bytes) | See description of value |

  * Pid = Project Id
  * Oid = Object Id
  * Rid = Revision Id

One other term which is used is Cid (Class Id), this is a short (2 bytes), and used as a shorter way (than a complete class-name) to reference classes.

Records are only added, never modified or deleted.

# Versioning

The image below is showing four revisions of a project. There are 3 tables: A, B and C.
The diagrams on top are showing the objects + relations for each revision. The number between the parenthesis is the Oid (Object Id).
The tables on the bottom are showing the complete database contents at the end of each revision. The number before the "." is the Object Id, the number after the dot is the Rid (Revision Id).

![db](https://github.com/opensourceBIM/BIMserver/raw/master/Documentation/img/db1.png)

# Value

Please read this first: [Link to description of EMF]
For all structural features of the class of the object, some bytes are written to the value part of the record. All values are written in the order defined by the EMF model and include all structural features of all super classes.

## Single attributes

| Type | Size(bytes) | Serialisation | Null representation |
| --- | --- | --- | --- |
| String | 2 + size of UTF-8 encoded bytes | UTF-8 encoded | -1 (as a short) |
| Integer | 4 | Default java serialisation | Cannot be null |
| Long | 8 | Default java serialisation | Cannot be null |
| Float |4 | Default java serialisation | Cannot be null |
| Double | 8 | Default java serialisation | Cannot be null |
| Boolean | 1 | 0 for false, 1 for true | Cannot be null |
| Date | 8 | Number of milliseconds since January 1, 1970, 00:00:00 GMT | -1 (as a long) |
| Tristate | 1 | 0 for true, 1 for false, 2 for undefined | Cannot be null |
| ByteArray |4 + Length | 4 bytes (int) for length + bytes | 0 (as an int) |
| Enum | 4 | Enum literal (int) | Cannot be null |

## Single references
A null reference is stored as:

| Short |
| --- |
| -1 |

A non-null reference is stored as:

| Short | Long |
| --- | --- |
| Cid | Oid |

## Multiple attributes
Multiple attributes (such as a list of integers) are stored inline. The first two bytes indicate the length of the list, after that all values are serialized like normal single attributes.

## Multiple references
Multiple references (lists of references to objects) are also stored inline. The first two bytes indicate the length of the list, after that all values are serialized like normal references.

## Example
Let's say we have two classes, Person and Company:

![personcompany](https://github.com/opensourceBIM/BIMserver/raw/master/Documentation/img/PersonCompany.png)

The Person class has Cid 1, the Company class has Cid 2.
Both classes have one instance, the person has Oid 100, the Company has Oid 101.

When serialized, the values will be

The person:

| Name | Age | Company |
| --- | --- | --- |
| (short)4 + 4 bytes | (int)80 | (short)2 + (long)101 |

The Company:

| Name | Employees |
| --- | --- |
| (short)9 + 9 bytes | (short)1 + (short)1 + (long)100 |