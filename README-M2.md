# Milestone 2 – JSON-java Library 


##  Code Changes

### 1. `toJSONObject(Reader reader, JSONPointer path)`
**Purpose:**  
Extracts the JSON subtree at the specified path using a streaming SAX parser.

**Highlights:**
- Parses the XML file incrementally.
- Stops parsing early when the target path is matched.
- Converts only the relevant subtree into a `JSONObject`.

**Example usage:**
```java
JSONPointer pointer = new JSONPointer("/contact/address/street");
JSONObject subTree = XML.toJSONObject(new FileReader("file.xml"), pointer);
```

---

### 2. `toJSONObject(Reader reader, JSONPointer path, JSONObject replacement)`
**Purpose:**  
Parses the entire XML into a `JSONObject`, navigates to the given path, and replaces the sub-object at that path with a provided `replacement`.

**Highlights:**
- Full XML is parsed into JSON.
- Uses `JSONPointer` for deep navigation and replacement.
- Ensures structure is preserved outside the replaced node.

**Example usage:**
```java
JSONPointer pointer = new JSONPointer("/contact/address/street");
JSONObject replacement = new JSONObject().put("street", "New Address");
JSONObject updated = XML.toJSONObject(new FileReader("file.xml"), pointer, replacement);
```

---

## Test Code Changes

New unit tests were added to `XMLTest.java` to verify correctness:

### 1. `testToJSONObjectWithJSONPointerExtract`
- Verifies that a valid path extracts only the targeted subtree.
- Asserts content of the extracted `JSONObject`.

### 2. `testToJSONObjectWithJSONPointerReplacement`
- Verifies that a replacement correctly overwrites the targeted subtree.
- Asserts the structure and values in the modified `JSONObject`.

### 3. Written 4 other unit test cases for testing error conditions.