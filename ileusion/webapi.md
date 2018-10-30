# ILEusion API documentation

## Starting ILEusion

All APIs are enabled when the ILEusion server is started. You can start the server (or multiple servers) using the `STRILESRV` command after installation. The command also has optional parameters.

```
ADDLIBLE ILEASTIC
ADDLIBLE NOXDB
ADDLIBLE ILEUSION
STRILESRV
```

All APIs are run within the same job. That means the job will run under the user that ran `STRILESRV` and will have the same authorities when accessing objects. The server will also have the same library list as the job that started the server.

## Web API documentation

All APIs accept and respond with JSON. All APIs will return an object with `success: false` if it fails, along with `message`. Not all APIs return with `success: true`.

APIs available:

* `/sql`
* `/call`
* `/dq/send`
* `/dq/pop`

---

### `/sql`

`/sql` allows you to run select statements.

**Example input**

```json
{
  "query": "select * from product where MANUID = 'SAMSUNG'"
}
```

**Example response**: Will return an array objects, with the key of the objects being the column name.

```
//Response here
```

---

### `/call`

`/call` allows you to call an ILE application or export function. The result is an array of values which is the values passed by reference from the application. Currently only program calls are supported.

**Request body**

The request body has three main attributes:

* `library` - string, name of library
* `object` - string, name of program (or service program if using `function` attribute)
* `function` - string, name of function (**optional**, only required when calling export functions in a service program)
* `result` - object defining the return type of the function (**optional**, only needed if return type is not void)
  * `type` - string, type of parameter: `int`, `uns`, `float`, `char`, `bool`, `ind`
  * `length` - number, should match length of type defined in the calling application (uses RPG sizes)
  * `arraysize` - number, size of array being returned (**optional**, only needed if functions returns an array)
* `args` - array of objects defining the parameters and their types:
  * `value` - string|number|bool
  * `values` - array, used if calling application has an array parameter. **Not to be used** at the same time as the `value` attribute
  * `type` - string, type of parameter: `int`, `uns`, `float`, `char`, `bool`, `ind`
  * `length` - number, should match length of type defined in the calling application (uses RPG sizes)

**Example request**

```
Dcl-Pi FAK100;
  pText Char(20);
  pNum1 Int(10);
  pNum2 Int(10);
  pNum3 Int(10);
End-Pi;
```

```json
{
	"object": "FAK100",
	"library": "BARRY",
	"args": 
  [
		{
			"value": "Text here",
			"type": "char",
			"length": 20
		},
		{
			"value": 11,
			"type": "int",
			"length": 10
		},
		{
			"value": 10,
			"type": "int",
			"length": 10
		},
		{
			"value": 0,
			"type": "int",
			"length": 10
		}
	]
}
```

**Example request (array)**

```
Dcl-Pi FAK101;
  pText Char(20);
  pNums Int(10) Dim(3);
End-Pi;
```

```json
{
	"object": "FAK101",
	"library": "BARRY",
	"args": 
  [
		{
			"value": "John",
			"type": "char",
			"length": 20
		},
		{
			"values": [
				3,
				666,
				5
			],
			"type": "int",
			"length": 10
		}
	]
}
```

---

### `/dq/send`

`/dq/send` can be used to push items into a data queue.

**Request body**

* `library` - string, name of library
* `object` - string, name of program
* `data` - any, data to be pushed
* `key` - string, key of the item to be pushed (**optional**)

**Example request**

```json
{
	"library": "BARRY",
	"object": "TESTDQ",
	"data": "Hello world!"
}
```

**Example response**

```json
{
  "success": true
}
```

---

### `/dq/pop`

`/dq/pop` can be used to pop an item from a data queue.

**Request body**

* `library` - string, name of library
* `object` - string, name of program
* `waittime` - number, `0` by default (**optional**)
* `key` - string|number, key of the item to be popped (**optional**)
* `keyorder` - key comparison, `EQ` by default (**optional**)

**Example request**

```json
{
	"library": "BARRY",
	"object": "TESTDQ"
}
```

**Example response**

```json
{
  "success": true,
  "data": "Hello world!"
}
```