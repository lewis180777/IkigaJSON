# IkigaJSON

IkigaJSON is a really fast JSON parser. It performed ~4x faster than Foundation in our tests when decoding a type from JSON.

### Adding the dependency

The 1.x versions are reliant on SwiftNIO 1.x, and for SwiftNIO 2.x support use the 2.x versions of IkigaJSON.

SPM:

```swift
.package(url: "https://github.com/Ikiga/IkigaJSON.git", from: "1.0.0"),
// Or, for SwiftNIO 2
.package(url: "https://github.com/Ikiga/IkigaJSON.git", from: "2.0.0"),
```

Cocoapods:

```cocoapods
pod 'IkigaJSON', '~> 1.0'
# Or, for SwiftNIO 2
pod 'IkigaJSON', '~> 1.0'
```

### Usage

```swift
import IkigaJSON

struct User: Codable {
    let id: Int
    let name: String
}

let data = Data()
var decoder = IkigaJSONDecoder()
let user = try decoder.decode(User.self, from: data)
```

### Raw JSON

IkigaJSON supports raw JSON types (JSONObject and JSONArray) like many other libraries do, alongside the codable API described above. The critical difference is that IkigaJSON edits the JSON inline, so there's no additional conversion overhead from Swift type to JSON.

```swift
var user = JSONObject()
user["username"] = "Joannis"
user["roles"] = ["admin", "moderator", "user"] as JSONArray
user["programmer"] = true

print(user.string)

print(user["username"].string)
// OR
print(user["username"] as? String)
```

### SwiftNIO support

The encoders and decoders support SwiftNIO.

```swift
var user = try JSONObject(buffer: byteBuffer)
print(user["username"].string)
```

We also have added the ability to use the IkigaJSONEncoder and IkigaJSONDecoder with JSON.

```swift
let user = try decoder.decode([User].self, from: byteBuffer)
```

```swift
var buffer: ByteBuffer = ...

try encoder.encodeAndWrite(user, into: &buffer)
```

The above method can be used to stream multiple entities from a source like a database over the socket asynchronously. This can greatly reduce memory usage.

### Performance

By design you can build on top of any data storage as long as it exposes a pointer API. This way, IkigaJSON doesn't (need to) copy any data from your buffer keeping it lightweight. The entire parser can function with only 1 memory allocation and allows for reusing the Decoder to reuse the memory allocation.

### Support

- All decoding strategies that Foundation supports
- Unicode
- Codable
- Escaping
- Performance 🚀
- Date/Data _encoding_ strategies
- Raw JSON APIs (non-codable)
- Codable decoding from `JSONObject` and `JSONArray`
- `\u` escaped unicode characters

TODO:

- Lightweight JSON inline comparison helpers

### Media

[Architecture](https://medium.com/@joannis.orlandos/the-road-to-very-fast-json-parsing-in-swift-4a0225c0313c)
