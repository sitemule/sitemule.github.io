### Parsing APIs

There are four APIs for parsing either JSON or XML documents:

* `json_ParseFile(String path)`
* `json_ParseString(String doc)`
* `xml_ParseFile(String path)`
* `xml_ParseString(String doc)`

For this tutorial we are going to use the JSON APIs. You will find that the JSON and XML APIs all share the same parameters, becuase under the covers they all point to the same `jx_` functions.

### Parsing a document

For this example, our file will contain the following JSON:

```json
{ 
	"price": 1234,
	"text" : "System & Method",
	"anObject" : {
		"country" : "Denmark"
	}
}
```

For our document, we are going to need a pointer for the handle.

```
Dcl-S jsonHandle Pointer;
```

We can read and parse the document with the `json_ParseFile` API, which returns a handle.

```
jsonHandle = json_ParseFile('simple.json');
```

`jsonHandle` will now point to the root of the document. We will use that pointer multiple times to access items within the JSON document. There are two ways to access data from the pointer:

Directly using a document path (like `/price` or `/anObject/country`):

```
Dcl-S price   Packed(11:2);
Dcl-S text    Char(50);
Dcl-S country Char(50);

price   = json_GetNum(jsonHandle: '/price');
text    = json_GetStr(jsonHandle: '/text');
country = json_GetStr(jsonHandle: '/anObject/country': 'N/A');
```

You can see a third parameter on `json_GetStr`, which is the default return value if the item in the document cannot be found.

Creating a handle to another part of the JSON document:


```
Dcl-S Node Pointer;
Dcl-S price Packed(11:2);

// Locate and return the value
Node  = json_Locate(jsonHandle: '/price');
price = json_GetNum(Node);
```

When you're finishing working with your document, **always** deallocate it with `json_Close`. (There is also `xml_Close`).

```
json_Close(jsonHandle);
```
