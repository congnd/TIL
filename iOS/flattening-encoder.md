Consider this data structure:

```Swift
struct B : Codable {
  var name: String
  var age: Int
}

struct A: Codable {
  var id: Int
  var attribute : B
}

let foo = A(id: 1, attribute: B(name: "name", age: 29))

let result = try! JSONEncoder().encode(foo)
let string = String(data: result, encoding: .utf8)!
print(string)
```

If you use the standard JSONEncoder, you will get this json as an output:
```
{
  "id": 1,
  "attribute": {
    "name": "name",
    "age": 29
  }
}
```

But sometimes you may want to flatten that json to get something like this:
```
{
  "id": 1,
  "name": "name",
  "age": 29
}
```

So what do you do? Well, we can simply use the standard encode protocol to do this.

By implement our own `encode(to:)` func, we can easily achieve the result.

```Swift
enum Keys: String, CodingKey {
  case id
}

func encode(to encoder: Encoder) throws {
  // Encode the `id` as normal
  var container = encoder.container(keyedBy: Keys.self)
  try container.encode(id, forKey: .id)
  
  // Put everything from `attribute` into current encoder without container.
  try attribute.encode(to: encoder)
}
```

Ok, now you get the json output as above.
