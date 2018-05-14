Protocol Buffers is one of the three available channels to access the methods of the [Service Interfaces](Service-Interfaces).

# Introduction

As the BIMserver's main function is to allow other software to store and retrieve information about building information models, it is very important to support a wide variety of programming languages. From the beginning of the BIMserver project a SOAP interface has been available, but SOAP has a few disadvantages:
  * SOAP is XML based, which makes it slower/larger
  * Almost every SOAP stack interprets the standards a little different
  * A lot of diversions/extensions are available, excluding certain soap clients

For these reasons another interface has been implemented: [Protocol Buffers](http://code.google.com/p/protobuf/).

The Protocol Buffers API maps all the functions available in the services [LINK], so the actual calls will not be described in this document.

# Proto file

The .proto file can be downloaded from a running BIMserver. Go to the "Info" page and click on the "Web Services" tab, then download the "Protocol Buffers Descriptor File". With this .proto file you can generate the service interface and messages for a wide range of programming languages, please check out the Protocol Buffers website for more details.

# BIMserver Specific

The "Service Interface" methods are not available in the Protocol Buffers API as-is because PB does have some limitations.

All service calls in Protocol Buffers allow only 1 parameter and 1 return type, for this reason the BIMserver generates specific "Request" and "Response" messages for every call. For example the "login" method uses the "LoginRequest" message as input parameter, and the "LoginResponse" message as the return type.

# Request Messages

All request messages contain the same amount of fields as there are parameters in the service call. So for the login method, the "LoginRequest" message contains 2 fields: "username" and "password", both of type "string".

# Response Messages

All response messages contain 2 fields. The first field is the "errorMessage" field of type "string". This field always contains "OKE" if the call succeeded, or something else describing the error if the call did not succeed. The reason for this construction is the fact that Protocol Buffers does not support Exceptions, but the "Service Interface" does throw them. The BIMserver converts the exceptions thrown to errorMessage values.

The second field is the actual return value. This is always the same value as in the ServiceInterface, or unset if an error occured. The special "Void" type is used for return-type-less methods.

# RPC

Protocol Buffers is only a message format, the actual transport method can be chosen by the developer. We have chosen to use [Protobuf Socket RPC](http://code.google.com/p/protobuf-socket-rpc/). This implementation only provides a Java and Python version for now, but as the implementation is pretty straight-forward other languages could be used without too much hassle.