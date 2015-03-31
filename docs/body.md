# iCargo body data structure

The body of all iCargo compliant requests and responses is a uniform JSON data structure using UTF-8 character encoding. This data structure can contain one or more unordered facts and a digital signature if applicable. The body shall be implemented as a single object or array.

Compliant to the JSON specification RFC7159, the following body data structure is expected:
```
fact	= object
body	= fact | array of 
			number value-separator 
			fact (value-separator fact)*
			[value-separator signature]
```
In case the body is an array, this array starts with the number of facts to provide preprocessing information before the whole data-structure is being read. The number also indicates at which position a digital signature can be expected. The digital signature is a hash-value of the number and the facts. Whether a signature should be present or not must be defined in advance including sharing the corresponding public-key and hashing algorithm to validate the signature. (see: “setting up a connection”)

A body with a single fact and a signature looks like:
```javascript
[ 
	1, 
	<fact>,	
	cf23df2207d99a74fbe169e3eba035e633b65d94
]
```
An example of a body with a single fact without a signature is include at the bottom of this section.

A fact has the following mandatory data structure:
```javascript
{
	subject:	<reference>,
	source:		<reference>,
	content:	<reference>
	time: 		<timestamp>	|| -1
	place:	 	<reference>	|| 0
	state: 		<reference>	|| 0
	value: 		<object>
}
``` 
___Notes:___

The timestamp is defined according to ISO 8601.

A reference is a value of the type string and refers to an entity or a concept. There are two types of references: 
* a unique identification code such as a UUID of an entity
* or a path containing the name (alias) of the entity or concepts and its parent to specify the scope. A path provides a more human readable string of information and is often used for concepts as a local unique reference.

The use of paths implies the necessity of an ontology explaining the meaning of the words used in the path to avoid misunderstanding. A unique identification code doesn’t have that problem because it is only a pointer to an entity or concept without further meaning.

Examples of valid references are:
```javascript
14bec851-528c-471b-e3cb-5328054a394c	  // UUID 
40536090246888177                         // GSIN used to identify a shipment 
software/access-point/eagle               // path to access point named “Eagle”
0	// refers to the concept "unknown"
``` 

## Request body
Providing information or requesting a specific service shall be done by posting one or more facts. These facts might contain information about entities or the requested service such as the criteria for a search request. The content attribute of a fact has an important role for routing and processing facts and should refer to a known concept in the semantic model of the recipient.  

## Result response body
All request result in a synchronous status response or a direct result response. Depending on the type of request, an asynchronous result response could follow later. These results will be returned as a request body by posting them to the requesting software service or application.

In all cases, the response contains one ore more facts.

## Status response body
Synchronous status responses provide validation information about a request whether it is accepted or rejected. Validation checks can be performed with little processing resources and therefore the response can be immediately. Originally, additional response information were specified as plain text. Using the same data structure for all responses is not only a more consistent design but also provides systematic and structured information for automatic processing of the responses in a uniform way. This is especially useful in case of automated testing of a software service that has impemented the iCargo REST API.   

An example of a succesful HTTPstatus 202 response body:
```javascript
{
	"subject": "9cdcb2c6-5e7d-4857-82d7-ed93c51daf3e",
	"source": "14bec851-528c-471b-e3cb-5328054a394c",
	"content": "response/info",
	"time": "2015-03-27T22:58:03.784Z",
	"place": "493638e7-f2fd-43b8-fb87-8b0486caee69",
	"state": "202",
	"value": {
		"description": "Request is accepted",
		"details": "Request is being processed."
	}
}
```
The content attribute refers in this case to concept "info" within the context of being a response state.  
