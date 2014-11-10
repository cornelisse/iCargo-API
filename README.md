![iCargo-API logo](images/iCargo-API-64.png) iCargo-API
==========

The iCargo REST API is an entity-centric HTTP REST interface with uniform HTTP-headers and a prescribed structure for the HTTP-body. The term “resources” is used for concepts and entities to be addressed. Concepts and entities must be described in a knowledge base which is an implementation of a specific version of a specific semantic model.

This specification describes the following topics:
*  Capabilities, an overview of the functionality;
*  Information model, required for addressing (REST) resources and describes how the resources and their status shall be modelled in a knowledge base; 
*  Generic specification, applicable for all API calls such as mandatory HTTP-header attributes and the path structure of the URL;
*  Access restrictions, authorisation requirements related to the different types and roles identified for an actor;
*  Function specific specification, a detailed description for each HTTP call of the path to a resource, how to use the header attributes and the expected result.

## Capabilities
The Entity-centric Services interface [IES] supports the following entity-centric specific capabilities of an Access Point:
*  To create and register an entity;
*  To search for an entity and discover which Access Points can provide information; 
*  To provide information about a concept or entity;
*  To perform an entity related action such as publish/subscribe;
*  To change or update the attributes an entity including relations to other entities;
*  To delete an entity.
Additionally, the following capabilities are supported as well:
*  To support invitations for cooperation;
*  To provide information about the Access Point itself;
*  To route information to other software services as part of a workflow;
* To support life cycle management of concepts and entities and their states.

Section E4 will elaborate how each capability is supported per API call.

