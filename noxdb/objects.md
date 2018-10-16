In the [second tutorial](/noxdb/parsing) of this series, we looked at parsing a JSON document and reading from an object. This tutorial will just extend do that.

---

With `json_SetIterator`, not only can you loop through arrays but you can also loop through objects too, to get a key/value list. See [this page](/noxdb/arrays) to learn about iterators in noxDB.

We are using two new APIs here:

* `json_GetName` which returns a string of current items key.
* `json_GetValue` which returns a string of the current items value. You can also use one of the many `json_Get*` functions available, like `json_GetStr`, `json_GetInt` or `json_GetBool`.

```
Dcl-S  Document Pointer;
Dcl-S  name     Varchar(10);
Dcl-S  value    Varchar(40);
Dcl-DS list     likeds(JSON_ITERATOR);

Document = JSON_ParseString (
  '{                      '+
  '  a:123,               '+
  '  b:"text",            '+
  '  c:"More text"        '+
  '}'
);

list = json_SetIterator(Document);
dow json_ForEach(list);
  name  = json_GetName(list.this);
  value = json_GetValue(list.this);
enddo;
```

---

Next, what if you have data which has objects within objects? Luckily `list.this` also allows you to access subnodes too.

You can use `json_GetValuePtr` to determine whether the current item in the list is a primitive value or an object.

```
Document = json_ParseString('{ a: 1, b : { c : 2 , d : 3 }}');

list = json_SetIterator(Document);
dow json_ForEach(list);
  name  = json_GetNameAsPath(list.this:'.');
  value = json_GetValue (list.this);

  //Check if item is an object.
  If (json_GetValuePtr(list.this:*Null) = *Null);
    Dsply (name + ' is an object');
    //Do work with list.this here.
  else;
    dsply (name + value);
  endif;

enddo;
```
