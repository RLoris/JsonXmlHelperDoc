# JsonXmlHelper

![JsonXmlHelper](./assets/icon288.png)

- UE plugin to handle Json and Xml manipulation and (de)serialization
- Async load a Json or Xml from file, string or URL (GET)
- (De)serialize struct, object, array, set, maps, int, float, byte, boolean, string... to/from Json and Xml
- Create / Read json value (parse and stringify)
- Create / Read Xml node (parse and stringify)
- Can be used in any blueprint

<br>

![Nodes](./assets/thumbnail.png)

[Link to the plugin in the marketplace](https://www.unrealengine.com/marketplace/en-US/product/69f737d5e17041079d05044e705f4bee)

# Documentation

<br>

# Json

![Json nodes](./assets/nodes1hd.png)

<br>

## Load JsonValue

![Json load](./assets/node1hd.png)

    Note: When using "LoadFromUrl", a GET request will be performed against the provided Url, then the result will be parsed into Json if valid

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| LoadJson | Options(JsonLoaderOptions) | Completed(JsonValue(JsonValueWrapper)), Failed(ErrorReason(String)) | Asynchronous load a JsonValue from (string or file or url), completed is called when the conversion was successful, failed otherwise with the error reason |

<br>

## Create JsonValue

![Json create](./assets/node2hd.png)

    Note: Do not use withespaces or special symbols in your struct to avoid deserialization/parsing errors, prefer using camelCase or PascalCase

    You can easily create nested objects/arrays using these nodes, carefull not to create loops in the json tree as it can cause a crash !

![Json create example](./assets/node11hd.png)

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| Parse | JsonString(String) | Result(Bool), JsonValue(JsonValueWrapper) | (Deserialize) Synchronous parse a string into a json value, use LoadJson for async capabilities |
| CreateJsonStringValue | Value(String) | Result(JsonValueWrapper) | Creates a json string from a string value |
| CreateJsonNumberValue | Value(float) | Result(JsonValueWrapper) | Creates a json number from a float value |
| CreateJsonNumberValue | Value(Int) | Result(JsonValueWrapper) | Creates a json number from an integer value |
| CreateJsonNumberValue | Value(Int64) | Result(JsonValueWrapper) | Creates a json number from an integer64 value |
| CreateJsonNumberValue | Value(Byte) | Result(JsonValueWrapper) | Creates a json number from a byte value |
| CreateJsonBooleanValue | Value(Bool) | Result(JsonValueWrapper) | Creates a json boolean from a bool value |
| CreateJsonNullValue | void | Result(JsonValueWrapper) | Creates a json null value |
| CreateJsonArrayValue | Values(Array(JsonValueWrapper)) | Result(JsonValueWrapper) | Creates a json value from an array of json values |
| CreateJsonObjectValue | Values(Map(String, JsonValueWrapper)) | Result(JsonValueWrapper) | Creates a json object from a map of json values |
| CreateJsonCustomValue | Value(AnyStruct) | Out(JsonValueWrapper) | Creates a json value from any struct provided |

<br>

## Query JsonValue

![Json query](./assets/node3hd.png)

    Note: To create a pattern query, use a field name or numeric value if the value is an array, separate the query elements by "/" : example: "MyField/0/OtherField/1", this will return a JsonValue located inside a field called MyField, then at the index 0 of the array, then inside a field called OtherField, then at the index 1 of the array inside OtherField !

    You can easily query nested JsonValue using this syntax pattern

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| GetType | void | Result(EJsonValueType) | Retuns the type of the value contained inside the JsonValue |
| IsType | Filter(EJsonValueType) | Result(Bool) | Returns true if the type matches the type of the value inside the JsonValue |
| HasField | Pattern(String), FilterType(EJsonValueType) | Result(Bool) | Checks recursively whether a specific key exists inside the JsonValue using a pattern |
| FindField | Pattern(String), FilterType(EJsonValueType) | Result(Bool), Value(JsonValueWrapper) | Finds recursively a specific key inside the JsonValue using a pattern |
| GetFieldNames | void | Result(Bool), FieldNames(Array(String)) | If JsonValue is an object, returns the field names of this object |
| Equals | Other(JsonValueWrapper) | Result(Bool) | Compare two JsonValue to find out if they are equal |

<br>

## Read JsonValue

![Json read](./assets/node4hd.png)

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| Stringify | PrettyPrint(Bool) | Result(Bool), JsonString(String) | (Serialize) Converts a JsonValue into a string, printing the value on multiple rows or not to save characters |
| GetStringValue | Default(String) | Result(Bool), Value(String) | Returns a string or default value if conversion is not valid |
| GetNumberValue | Default(Float) | Result(Bool), Value(Float) | Returns a float or default value if conversion is not valid |
| GetNumberValue | Default(Int) | Result(Bool), Value(Int) | Returns an int or default value if conversion is not valid |
| GetNumberValue | Default(Int64) | Result(Bool), Value(Int64) | Returns an int64 or default value if conversion is not valid |
| GetNumberValue | Default(Byte) | Result(Bool), Value(Byte) | Returns a byte or default value if conversion is not valid | 
| GetBooleanValue | Default(Bool) | Result(Bool), Value(Bool) | Returns a boolean or default value if conversion is not valid |
| IsNullValue | void | bool | Returns true if the JsonValue contains null |
| GetArrayValue | void | Result(Bool), Values(Array(JsonValueWrapper)) | Returns an array filled with JsonValue if conversion is valid |
| GetObjectValue | void | Result(Bool), Values(Map(String, JsonValueWrapper)) | Returns a map filled with field name as key and JsonValue as value if conversion is valid |
| GetCustomValue | void | Success(Bool), Value(AnyStruct) | Tries to fill the provided struct with the JsonValue if conversion is possible |

<br>

## Update JsonValue

![Json update](./assets/node5hd.png)

    Note: Carefull not to create loops in the json tree as it can cause a crash !

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| AddField | FieldName(String), FieldValue(JsonValueWrapper) | Success(Bool), Result(JsonValueWrapper) | If the JsonValue is an object, this adds a new field, and returns the updated version or nullptr if fail |
| RemoveField | FieldName(String) | Success(Bool), Result(JsonValueWrapper) | If the JsonValue is an object, this removes a new field, and returns the updated version or nullptr if fail |
| AddValue | Value(JsonValueWrapper), Index(Int) | Success(Bool), Result(JsonValueWrapper) | If the JsonValue is an array, this adds a new value, and returns the updated version or nullptr if fail |
| RemoveValue | Value(JsonValueWrapper) | Success(Bool), Result(JsonValueWrapper) | If the JsonValue is an array, this removes a value, and returns the updated version or nullptr if fail |
| RemoveValueAt | Index(Int) | Success(Bool), Result(JsonValueWrapper) | If the JsonValue is an array, this removes a specific index value, and returns the updated version or nullptr if fail |

<br>

# Xml

![Xml nodes](./assets/nodes2hd.png)

## Load XmlNode

![xml load](./assets/node6hd.png)

    Note: When using "LoadFromUrl", a GET request will be performed against the provided Url, then the result will be parsed into Xml if valid

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| LoadXml | Options(XmlLoaderOptions) | Completed(XmlNode(XmlNodeWrapper)), Failed(ErrorReason(String)) | Asynchronous load an XmlNode from (string or file or url), completed is called when the conversion was successful, failed otherwise with the error reason |

<br>

## Create XmlNode

![Xml create](./assets/node7hd.png)

    Note: Do not use withespaces or special symbols in your struct to avoid deserialization/parsing errors, prefer using camelCase or PascalCase

![Xml create example](./assets/node12hd.png)

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| Parse | XmlString(String) | Success(Bool), Result(XmlNodeWrapper) | (Deserialize) Synchronous parse a string into an XmlNode, use LoadXml for async capabilities | 
| CreateCustomXmlNode | Value(AnyStruct) | Valid(Bool), Result(XmlNodeWrapper) | Converts any struct to an Xml node if it's possible |
| CreateXmlNode | TagName(String) | Valid(Bool), Result(XmlNodeWrapper) | Creates a new Xml Node with a valid tag name or nullptr if fail |

<br>

## Query XmlNode

![Xml query](./assets/node8hd.png)

    Note: To create a pattern query, use a tag name or numeric value, separate the query elements by "/" : example: "MyTag/0/NestedTag/1", this will return an XmlNode located inside a node called MyTag, then at the index 0 of the array, then inside a node called NestedTag, then at the index 1 of the array inside NestedTag !

    You can easily query nested XmlNode using this syntax pattern

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| IsRootNode | void | Result(Bool) | Checks whether the current node is the root node of the xml tree |
| IsLeafNode | void | Result(Bool) | Checks whether the current node is a leaf node in the xml tree |
| GetRootNode | void | Result(XmlNodeWrapper) | Returns the root node of the current node |
| GetParent | void | Result(Bool), Parent(XmlNodeWrapper) | Returns the parent node if there is one |
| GetChildren | void | Children(Array(XmlNodeWrapper)) | Returns the children nodes |
| GetChildrenCount | void | Result(Int) | Returns the amount of children this node has |
| GetChildrenTags | void | Tags(Array(String)) | Returns the name tags of the children this node has |
| GetFirstChild | void | Result(Bool), Child(XmlNodeWrapper) | Returns the first child of the current node if there is one |
| GetLastChild | void | Result(Bool), Child(XmlNodeWrapper) | Returns the last child of the current node if there is one |
| GetNext | void | Result(Bool), Next(XmlNodeWrapper) | Returns the next sibling of the current node if there is one |
| GetPrev | void | Result(Bool), Prev(XmlNodeWrapper) | Returns the previous sibling of the current node if there is one |
| HasNode | Pattern(String) | Result(Bool) | Checks recursively whether a specific node exists using a search pattern |
| FindNode | Pattern(String) | Result(Bool), Node(XmlNodeWrapper) | Finds recursively a specific node using a search pattern |
| IsAttached | Other(XmlNodeWrapper) | Result(Bool) | Checks whether the current node is attached to the other node |

<br>

## Read XmlNode

![Xml read](./assets/node9hd.png)

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| Stringify | PrettyPrint(Bool) | Result(Bool), XmlString(String) | (Serialize) Converts a XmlNode into a string, printing the value on multiple rows or not to save characters |
| GetAttribute | Name(String) | Result(Bool), Value(String) | Gets the attribute value with a specific name if it exists |
| GetAttributes | void | Attributes(Map(String, String)) | Gets all attributes of the current node |
| GetTag | void | Result(String) | Returns the tag of the current node |
| GetContent | void | Result(String) | Returns the content of the current node |
| GetCustomValue | void | Success(Bool), Value(AnyStruct) | Converts an Xml Node to any struct if it's possible |

<br>

## Update XmlNode

![Xml update](./assets/node10hd.png)

| Node | Inputs | Outputs | Note |
| ---- | ------ | ------- | ---- |
| AddAttribute | Name(String), Value(String) | Result(Bool), This(XmlNodeWrapper) | Adds an attribute if the name is valid, returns this node for chaining |
| SetTag | Name(String) | Result(Bool), This(XmlNodeWrapper) | Updates the tag name if the new name is valid, returns this node for chaining |
| SetContent | Content(String) | This(XmlNodeWrapper) | Sets the content of the node, returns this node for chaining |
| AddChild | Child(XmlNodeWrapper), Index(Int) | Result(Bool), This(XmlNodeWrapper) | Adds a child to the current node at index, returns this node for chaining |
| AddNext | Next(XmlNodeWrapper) | Result(Bool), This(XmlNodeWrapper) | Adds a next sibling to the current node, returns this node for chaining |
| AddPrev | Prev(XmlNodeWrapper) | Result(Bool), This(XmlNodeWrapper) | Adds a previous sibling to the current node, returns this node for chaining |
| Detach | void | Result(Bool), This(XmlNodeWrapper) | Detaches the current node from it's parent, returns this node for chaining |