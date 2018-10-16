noxDB not only supplies way to read and parse JSON/XML, but also ways to generate it.

There are four main APIs that we're going to use to generate an array of objects from a database table using embedded SQL.

* `json_NewArray` which will return a pointer to a new instance of an array.
* `json_newObject` which will return a new instance of an object, which will be used for each row.
* `json_SetNum` which will create a new key/value (numeric) node within an object.
* `json_SetStr` which will create a new key/value (string) node within an object.
* `json_ArrayPush` which will push an item to an existing array.
* `json_AsJsonText` which will take a pointer and generate a JSON document out of it for us.

---

```
Dcl-S ArrayPtr  Pointer;
Dcl-S ObjectPtr Pointer;
Dcl-S Result    Char(4098);

ArrayPtr = json_NewArray();

EXEC SQL
  DECLARE Prod_Cur CURSOR FOR
  SELECT * FROM PRODUCT
  WHERE MANUID = 'SAMSUNG';

EXEC SQL OPEN Prod_Cur;

If (SQLSTATE = '00000');
  EXEC SQL FETCH Prod_Cur INTO :PRODUCT;

  Dow (SQLSTATE = '00000');
    ObjectPtr = json_newObject();
    json_SetNum(ObjectPtr:'key': PRODUCT.ProdKey );
    json_setStr(ObjectPtr:'id': %TrimR(PRODUCT.ProdID) );
    json_setStr(ObjectPtr:'desc': %TrimR(PRODUCT.DESC) );
    json_SetNum(ObjectPtr:'price': PRODUCT.Price );
    json_ArrayPush(ArrayPtr:ObjectPtr);

    EXEC SQL FETCH Prod_Cur INTO :PRODUCT;
  Enddo;

Endif;

EXEC SQL CLOSE Prod_Cur;

Result = json_AsJsonText(ArrayPtr);
json_NodeDelete(ArrayPtr);
```

---

The good news is that there is a way to make this even simpler. noxDB ships with native SQL functions to do all this kind of work for you.

We are going to use the `json_sqlResultSet` function to **achieve the same result**, but with less lines of code.

```
Dcl-S ArrayPtr Pointer;  
Dcl-S sql Varchar(256);  
Dcl-S Result Char(4098);  

//------------------------------------------------------------- *

// return one object with one row
sql = (  
  'select * ' +  
  'from product ' +  
  'where MANUID = ''SAMSUNG'''  
);  

// The key is setup in the json template:  
ArrayPtr = json_sqlResultSet(sql);  

Result = json_AsJsonText(ArrayPtr);

// Cleanup: delete the object and disconnect  
json_NodeDelete(ArrayPtr);  
json_sqlDisconnect();
```

---

There are plenty more SQL APIs available in noxDB and you can find the [documentation for them here](/noxdb/docs/jsonsql).
