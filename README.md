# Porygon

![Travis Build Status](https://img.shields.io/travis/rhysforyou/Porygon?style=flat-square)
![Swift Package Manager compatible](https://img.shields.io/badge/SPM-compatible-blue?style=flat-square)
![Supports macOS, iOS, tvOS, watchOS, and Linux](https://img.shields.io/badge/platform-macOS%20|%20iOS%20|%20tvOS%20|%20watchOS%20|%20Linux-blue?style=flat-square)

Porygon is a networking library heavily inspired by [tiny-networking](https://github.com/objcio/tiny-networking), but designed primarily for use with Combine. It defines an `Endpoint` type which encapsulates the relationship between a `URLRequest` and the `Decodable` entity it represents.

## A Simple Example

```swift
struct Repository: Decodable {
    let id: Int64
    let name: String
}

func repository(author: String, name: String) -> Endpoint<Repository> {
    return Endpoint(json: .get, url: URL(string: "https://api.github.com/repos/\(author)/\(name)")!)
}

let endpoint = repository(author: "rhysforyou", name: "Porygon")
```

This simply gives us the description of an endpoint, to actually load it, we need to subscribe to its publisher:

```swift
subscriber = URLSesion.shared.endpointPublisher(endpoint)
    .sink(receiveCompletion: { print("Completed: \($0)") },
          receiveValue: { print("Value: \($0)") })
```

If the subscriber is cancelled or deallocated before it finishes, any networking operations will be halted.
