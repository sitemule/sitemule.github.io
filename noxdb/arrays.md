### Looping through arrays

Arrays are important and the nice thing about working with arrays in noxDB/JSON is that there is no fixed limit on length.

noxDB has built in iterators to loop through arrays and objects easily. Both the JSON and XML APIs have this capability.

There are two main APIs we're going to use here:

* `json_SetIterator` - which returns a data-structure containing info about your iterator. There is also `xml_SetIterator`.
* `json_ForEach` - which will loop through the iterator that you created and update your iterator. There is also `xml_ForEach`. This function returns an indicator when the next item has been found.

---

```
Dcl-S  Document Pointer;
Dcl-S  value    Varchar(40);
Dcl-DS list     likeds(JSON_ITERATOR);

// List elements:
Document = json_ParseString('["a","b","c"]');

// Using an iterator
list = json_SetIterator(Document);
dow json_ForEach(list); //Will stop looping when no more items have been found.
  value = json_GetStr(list.this);
enddo;
```

---

The `JSON_ITERATOR` structure has lots of useful subfields which you can make use of in your program:

* `list.this` is a pointer to the current element. If your array was a list of objects, you could use this pointer to find sub-nodes.
* `list.isFirst` is an indicator which is `*ON` if it's the first item in the array.
* `list.isLast` is an indicator which is `*ON` if it's the last item in the array.
* `list.count` is the current index of which the iterator is on.
