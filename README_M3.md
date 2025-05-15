
# XML to JSON Converter with Key Transformation  
**Milestone 3 - SWE 262 Project**

---



This library provides enhanced functionality to convert XML into JSON   

- Supports client-defined key transformations using Java Functional Interfaces.  
- Performs transformations *during parsing*, improving efficiency over post-processing.  
- Backward-compatible with the original `org.json.XML` class.

---


##  How to Compile and Run  

### 1. Compile the Project  
```bash
javac -d . org/json/XML.java org/json/junit/XMLTest.java
```

### 2. Run Unit Tests  
```bash
java -cp .:junit-4.13.2.jar:hamcrest-core-1.3.jar org.junit.runner.JUnitCore org.json.junit.XMLTest
```

---

## Changes  

###  **New Overloaded Method Added**  

```java
static JSONObject toJSONObject(Reader reader, Function<String, String> keyTransformer);
```

- `reader`: XML input as a `Reader` object.  
- `keyTransformer`: A Java `Function<String, String>` that defines how keys should be transformed.

---

###  **Usage Examples**

#### 1. Prepend "swe262_" to All Keys

```java
Function<String, String> prependTransformer = key -> "swe262_" + key;
String xml = "<root><child>value</child></root>";

JSONObject result = XML.toJSONObject(new StringReader(xml), prependTransformer);
System.out.println(result.toString(2));
```

**Output:**
```json
{
  "swe262_root": {
    "swe262_child": "value"
  }
}
```

#### 2️. Reverse All Keys

```java
Function<String, String> reverseTransformer = key -> new StringBuilder(key).reverse().toString();
String xml = "<root><child>value</child></root>";

JSONObject result = XML.toJSONObject(new StringReader(xml), reverseTransformer);
System.out.println(result.toString(2));
```

**Output:**
```json
{
  "toor": {
    "dlihc": "value"
  }
}
```

---

## Performance Considerations  

- **In-Library Transformation (Current Implementation):**
    - Efficient and performed during parsing.
    -  Eliminates the need for additional traversals after parsing.
    - Reduces time and space complexity for large datasets.

- **Client-Side Transformation (Milestone 1 Approach):**
    -  Requires an additional pass after JSON conversion.
    -  Inefficient for large JSON structures.
    -  Increased memory consumption due to intermediate results.

---

##  How to Create Custom Transformers  

Clients can easily create and pass any key transformation logic using Java’s `Function<String, String>` interface.

**Example: Make all keys uppercase.**

```java
Function<String, String> toUpperCase = String::toUpperCase;
```

No changes to the library code are needed—just pass the transformer as an argument!

---

