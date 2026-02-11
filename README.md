# ImplicitJSON
A wrapper for Swift's JSONSerializer, made to significantly reduce nesting and code complexity.

## Examples

Lets say you're trying to read from this JSON string below
```json
{
    "example" : "woah",
    "imNested" : {
        "thisIsData" : 30
    }
}
```

Reading all of these values from the JSON table normally in Swift would look something like this:

```swift
import Foundation

let jsonString = """
{
    "example" : "woah",
    "imNested" : {
        "thisIsData" : 30
    }
}
"""

if let data = jsonString.data(using: .utf8) {
    do {
        if let json = try JSONSerialization.jsonObject(with: data) as? [String: Any] {
            let nested = json["imNested"] as? [String: Any]
            
            print(json["example"] as? String)      // woah
            print((nested?["thisIsData"] as? Int) ?? -1)      // 30
        }
    } catch {
        print("JSON parsing error:", error)
    }
}
```

A little too long for my liking,
To read the same data using ImplicitJSON, the code would look like this:

```
let data = """
{
    "example" : "woah",
    "imNested" : {
        "thisIsData" : 30
    }
}
"""

import Foundation
            
let json = ImplicitJSON(json: data)

print(json.index(key: "example").value) // woah
print(json.index(key: "imNested").value) // all of the table contents
print(json.index(key: "imNested").index(key: "thisIsData").value) // 30
```

Hooray!

## What if a value does not exist?
For reading convenience, this system will silently fail--sort of. 
All error messages (for example, if something fails to read) will be passed to all children who attempt to index any values
This is intentional, and is designed to allow you to see what key failed to read, without any extra stress or work (hopefully..)

Thanks for coming to my ted talk.

