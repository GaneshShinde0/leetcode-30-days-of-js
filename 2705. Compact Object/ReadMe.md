### Solution Overview

In this problem, we are tasked with creating a JavaScript function `compactObject` that receives a JSON object as input and returns a "compact" version of that object. A "compact" object removes all keys associated with falsy values (e.g., `false`, `0`, `null`, `undefined`, etc.) from both top-level and nested structures, including arrays, which are treated like objects with numeric keys. The challenge here involves traversing through each level of the object, identifying falsy values, and removing them while preserving the nested structure.

### Key Concepts

- **Falsy values in JavaScript**: These include `false`, `0`, `-0`, `''`, `null`, `undefined`, and `NaN`. If a value is falsy, it will be excluded from the final compacted object.
- **Handling Arrays and Objects**: Both arrays and objects need to be processed differently. Arrays use numeric indices as keys, while objects use strings. The function should ensure both types are handled properly.
- **Nested Objects**: The function must recursively handle objects that may contain nested arrays or other objects.

### Use Cases for `compactObject`

1. **Data Cleaning**: This function is useful for removing unnecessary or invalid data (e.g., null or empty values) from JSON objects before further processing or storage.
2. **API Response Processing**: Third-party APIs often return keys with falsy values. Compacting these objects ensures that subsequent logic only handles meaningful data.
3. **UI Rendering**: When data is used to populate a user interface, `compactObject` can remove null or empty values to prevent errors or blank UI elements.
4. **Optimizing Storage**: Before saving data, compacting it reduces the storage size by eliminating keys with no value.

### Recursive Depth-First Search (DFS) Approach

**Intuition**: The recursive approach uses Depth-First Search (DFS) to traverse the object. For each object or array, it processes all keys/indices and rebuilds the object without any falsy values.

#### Algorithm
1. **Base Case**: If the value is falsy, ignore it.
2. **Arrays**: Recursively process each array element. Keep only truthy elements.
3. **Objects**: Recursively process each key-value pair in an object, removing keys with falsy values.
4. **Return**: Rebuild the compacted object or array.

#### Time Complexity
- **Time Complexity**: O(N), where N is the total number of elements (including nested ones) in the object.
- **Space Complexity**: O(D), where D is the depth of the object (determined by recursion stack depth).

### Iterative Depth-First Search (DFS) Approach

**Intuition**: Instead of relying on recursion (which can cause stack overflow for deeply nested objects), the iterative approach uses a stack to manually manage the DFS traversal.

#### Algorithm
1. **Initialization**: Use a stack to keep track of objects to process.
2. **Iterate**: For each object popped from the stack, process its keys or array indices. Add valid non-falsy values to a new object or array.
3. **Stack Management**: Whenever a nested object or array is found, push it onto the stack for further processing.
4. **Return**: The final compacted object or array.

#### Time Complexity
- **Time Complexity**: O(N), same as the recursive approach.
- **Space Complexity**: O(N), since the stack holds elements for traversal.

### Example Use of `compactObject`
```javascript
function compactObject(obj) {
    if (!obj || typeof obj !== 'object') {
        return obj;
    }

    if (Array.isArray(obj)) {
        return obj
            .map(compactObject)
            .filter(Boolean); // remove falsy values
    }

    return Object.keys(obj).reduce((acc, key) => {
        const value = compactObject(obj[key]);
        if (Boolean(value)) {
            acc[key] = value;
        }
        return acc;
    }, {});
}
```

This function works by recursively applying the logic to remove falsy values and rebuilding the structure while preserving the order and key-value relationships. For arrays, it filters out falsy values, and for objects, it only adds truthy values to the final result.

### Conclusion

The `compactObject` function is a valuable utility for cleaning JSON objects and ensuring they are free from unnecessary or invalid data. Whether using the recursive or iterative approach, both methods achieve linear time complexity and can handle various nested structures efficiently. The choice between recursion and iteration depends on your system's memory constraints and input size.
