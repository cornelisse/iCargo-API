# iCargo body data structure

The body of all iCargo compliant requests and responses is a uniform JSON data structure using UTF-8 character encoding. This data structure can contain one or more unordered facts and a digital signature if applicable. The body shall be implemented as a array.

Compliant to the JSON specification RFC7159, the following body data structure is expected:
```
fact	= object
body	= array of 
			number value-separator 
			fact (value-separator fact)*
			[value-separator signature]
```
A body starts with the number of facts to provide preprocessing information before the whole data-structure is being read. The number also indicates at which position a digital signature can be expected. The digital signature is a hash-value of the number and the facts. Whether a signature should be present or not must be defined in advance including sharing the corresponding public-key and hashing algorithm to validate the signature. (see: “setting up a connection”)

A body with a single fact looks like:
```javascript
[ 
	1, 
	<fact>,	
	cf23df2207d99a74fbe169e3eba035e633b65d94
]
```

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
```
14bec851-528c-471b-e3cb-5328054a394c	  // UUID 
40536090246888177                         // GSIN used to identify a shipment 
software/access-point/eagle               // path to access point named “Eagle”
``` 
A reference value equal to zero refers to the concept “unknown”.
