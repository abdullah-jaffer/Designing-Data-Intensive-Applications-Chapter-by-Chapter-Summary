# Chapter 4: Encoding and Evolution

- Software needs to be evolvable.
- Relational databases allow schema changes
- Document databases are schema less so we can have new old  and schema at the same time

## Backward and forward compatibility
When data or its structure changes we often need to make the corresponding change in application code. 
In server side or backend application we can have rolling upgrade/stage rollout. Rolling upgrade is introducing the new changes to a few nodes at a time to verify it is running smoothly.
In client side code we are at the whim of the client to upgrade. In web apps we have more control.
So it is possible that old and new version of our code can exist at the same time.

## Thus this leads to two options
- Backward compatibility: Newer code can read data that was written by older code.
- Forward compatibility: Older code can read data that was written by newer code.
 Backword compatibility is easier to implement because we know the old data structure.
Forward compatibility is trickier because we need to ignore newer changes without having prior knowledge of what that could be.
We look at different encoding formats and how they handle old and new code, like JSON, XML, Protocol Buffers, Thrift, and Avro.

## Formats for encoding data
1. In memory data is kept in data structures like lists, hashmaps, and trees
2. Data that needs to be sent written to a file or sent over a network needs to be encoded as a self-contained sequence of bytes’ like a JSON document. Sequence of bytes are different then data structures in memory.
Thus we need to translate this data from memory data structures to a byte sequence. This is called encoding.
## Language specific formats
There are certain language specific formats. Pickle for python, Java.io.Serializable for JAVA.

## Disadvantages
1. These serializations are not language specific.
2. While decoding we need certain arbitrary classes and data structures to put the information in, this can be a security issue.
3. No good support for versioning and forward/backward compatibility.

## JSON, XML and Binary formats
Standardized encoders such as JSON and XML are common. XML is verbose and unnecessarily complicated. JSON has built in browser support.
CSV is also popular and language independent but less powerful.

JSON, XML and CSV are human readable but they have problems.

1. Number ambiguity is an issue in XML and CSV, these formats do not differentiate string digits and numbers. JSON does, but it does not distinguish integers and floating point numbers. IEEE 754 floating point cannot represent 2^53 integers.
So numbers become inaccurate in languages that use floating point numbers like JavaScript. 
Twitter tweet id is 2^53 and it handles that by sending it as a floating point number and string.
2. XML and JSON support Unicode character strings but they don't support data in binary format so we have to convert binary date into base64 strings to send it in these formats. This is hacky and increases data size by 33%.
3. CSV does not support a schema. it does not have well defined rules such as what to have as a delimiter.

## Binary encoding
For internally used data that does not need to meet bottom line but needs to be fast, we can use binary encoding.
Distinguishing integers and floating-point numbers, or adding support for binary strings.
In particular, since they don’t prescribe a schema, they need to include all the object field names within the encoded data.
MessagePack, a binary encoding for JSON is quite popular

Encoding the following json
```
{
"userName": "Martin",
"favoriteNumber": 1337,
"interests": ["daydreaming", "hacking"]
}
```

takes about 66 bytes. Which is only a little less than 81 bytes taken by the textual JSON.
we will see how we can do much better, and encode the same record in just 32 bytes.

