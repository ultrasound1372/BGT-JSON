# BGT JSON

This repository provides an almost spec compliant JSON tree parser in pure BGT, to be used in BGT games.

## Features

* Consistency of types in accordance with the JSON specification. Note that numbers are represented by doubles.
* Conversion of string escapes defined in the JSON specification.
* Raw parser functions for when one wishes to parse a JSON string in the most efficient way possible, with the assumption that one knows exactly what will always be present. Useful enough for in-game communication.
* Object tree representation of JSON data that can be viewed and manipulated.
* Full nesting support up to memory limit in object tree.
* loads and dumps functions that allow one to convert between a JSON string and a JSON object tree.
* Convenience functions that ease the procedural creation of JSON object trees to be dumped.
* Produced and consumed JSON should be compatible with existing JSON APIs.

## Limitations

* Not compliant to JSON spec in the lack of UTF-8 support, since BGT is not unicode aware.
  * Does not support \u and \U escapes.
  * Odd behavior may result when interacting with JSON endpoints in the wild on some systems.
* Object tree overhead and speed of construction makes it unadviseable to use the tree implementation rapidly and continuously. BGT may leak objects.

## Usage

```
#include "JSON.bgt"
void main()
{
json_object o;
o.set("foo", "bar");
o.set("life", 42);
o.set("has_cookie", false);
json_value@[] vals;
vals.insert_last(JSON::make_value("This"));
vals.insert_last(JSON::make_value("exists"));
vals.insert_last(JSON::make_value(9000));
o.set("arr", vals);
json_object o2;
o2.set("bar", "bas");
o2.set("diddly", "squat");
o.set("obj", o2);
string jsondata=JSON::dumps(o, true);
clipboard_copy_text(jsondata);
json_object@ no=JSON::loads(jsondata);
if(no.type("obj")==JSON_OBJECT)
{
json_object@ no2=o.get_obj("obj");
if(no2.type("bar")==JSON_STRING)
{
string bar;
no2.get("bar", bar);
alert("obj.bar",bar);
}
}
if(no.type("arr")==JSON_ARRAY)
{
json_value@[]@ arr;
no.get("arr", arr);
if(arr[0].type==JSON_STRING)
{
string arr0;
arr[0].get(arr0);
alert("arr[0]", arr0);
}
}
}
```

See comments in JSON.bgt for further details.