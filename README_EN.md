

BIMserver
=========

The Building Information Model server (short: BIMserver) enables you to store and manage the information of a construction (or other building related) project. Data is stored in the open standard IFC. The BIMserver is not a fileserver, but it uses a model-driven architecture approach. This means that IFC data is stored in an underlying database. The main advantage of this approach is the ability to query, merge and filter the BIM-model and generate IFC files on the fly.

Thanks to it's multi-user support, multiple people can work on their own part of the model, while the complete model is updated on the fly. Other users can get notifications when the model (or a part of it) is updated. Furthermore the BIMserver will warn you when other users have changed something in the model while you were editing.

BIMserver is built for developers. We've got a great wiki on https://github.com/opensourceBIM/BIMserver/wiki and are very active supporting developers on https://github.com/opensourceBIM/BIMserver/issues 

See a full list of features on http://www.bimserver.org/ 

Licence: GNU Affero General Public License, version 3 (see http://www.gnu.org/licenses/agpl-3.0.html)
Beware: this project makes intensive use of several other projects with different licenses. Some plugins and libraries are published under a different license.

Get Started
-----------

    Quick Guide
    Requirements Version 1.2
    Requirements Version 1.3
    Requirements Version 1.4
    Requirements Version 1.4 > 2015-09-12
    Requirements Version 1.5
    Download
    JAR Starter
    Setup

Deployment

    Ubuntu installation 1.3
    Windows installation
    Security
    Memory Usage
    More memory
    Performance statistics
    Large databases

Developers

    Service Interfaces
        SOAP
        JSON
        Protocol Buffers
    Common functions
        Checkin
        Download
    Data Model
        SProject
        SRevision
    Low Level Calls
    Endpoints

Clients

    Java Client
        Java Client (Maven)
    PHP Client
    JavaScript Client

BIMServer Developers

    Plugins in 1.5
    Plugin Development
        Serializer Plugin
            XSLT Serializer
            BIMsurfer Serializers
        Deserializer Plugin
        Model Compare Plugin
        Model Merge Plugin
        Query Engine Plugin
        Render Engine Plugin
        ObjectIDM Plugin
        Schema Plugin
        Service Plugin
    Eclipse
    Eclipse Modeling Framework
    Embedding
    Terminology
    Database/Versioning
    IFC STEP Encoding
    Communication
    Global changes in 1.5
    Writing a service
    Services/Notifications
    BIMserver 1.5 Developers
    Extended data
    Extended data schema
    Object IDM

New developments

    New remote service interface
    Plugins new
    Deprecated
    New query language
    Visual query language
        Contains
        Decomposes
        DecomposesContains
        Properties
        Reusable
    Reorganizing BIMserver JavaScript API

General

    FAQ
    Known Issues
        Problems querying IFC
        Web Sockets
    License
    Feature statusses
    Roadmap

