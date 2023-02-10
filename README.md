<div align="center">
<h1>Model-View architecture</h1>

<p>
<strong>A more natural architectural approach on writing SwiftUI applications</strong>
</p>
</div>

### Table of Contents

- [Introduction](#introduction)
  - [Layers breakdown](#layers-breakdown)
    - [Model](#model)
    - [View](#view)
  - [MV vs MVVM](#mv-vs-mvvm)
- [Deep Dive](#deep-dive)
- [Credits](#credits)

## Introduction

**Model-View architecture** is a software design pattern that separates the representation of data from the presentation of data. In this architecture, the model represents the data and define the logic that manipulates this data, while the view is responsible for presenting the data to the user. This pattern provides more natural way to write an application on declarative framework *(SwiftUI)*, eliminating unnecessary complexity and extra work for no visible benefits.

### Layers breakdown

#### Model

The entities in the `Model` layer are not a simple structs (`DTOs` or `POJOs`), they serve as repositories for the application's data and embody the logic for manipulating that data. This domain is a system of such objects, that has their own properties, methods and relationships. Models should be independent and could be reused across all modules of an app. The objects in the Model layer could use Active Record, Data Mapper or Factory Method design patterns to avoid doing extra work and creating unnecessary layers in the project.

<details>
<summary>Example</summary>

```swift
struct Product: Identifiable, Codable {
    let id: String
    let name: String
    let price: Double

    static var all: [Product] { 
        get async {
            // ...
        }
    }
}
```

</details>

#### View

The `View` layer contains entities that provides user interaction and handles presentation logic. Thanks to the framework the binding to the `Model` is performed by only using `@State` property wrapper and most of the other work that was done by `ViewModel` is processed automatically by the SwiftUI. You could also use the composable nature of the SwiftUI's `View` and divide your entity into smaller parts for better maintainability. To facilitate the sharing of state/model between multiple Views, the use of `@EnvironmentObject` is recommended.

<details>
<summary>Example</summary>

```swift
struct ProductList: View {
    @State private var products: [Product] = .init()

    var body: some View {
        List(products) {
            // ...
        }
        .task {
            self.products = await Product.all
        }
    }
}
```

</details>

### MV vs MVVM

**Model-View-ViewModel (MVVM)** is a commonly proposed architecture for SwiftUI. However, in practice, the use of this pattern may lead to unwanted complexity of the codebase. So you may find yourself in a position of thinking that you are fighting the framework. And it is true. In SwiftUI the implementation of the `View` component contains support for bindings through the utilization of the `@State` property wrapper. This feature obviates the necessity for the implementation of a separate `ViewModel`, as the `View` effectively serves as a `ViewModel`.

On the other hand, in the **Model-View (MV)** architecture the `Model` store and process data, while `View` binds to this model and renders updates to the user. This approach effectively utilizes the advantage of the inherent characteristics of the framework, leading to a seamless and intuitive development experience.

## Deep Dive

More covered topics and detailed information about **Model-View (MV) architecture** could be found in the Wiki.

## Credits

Appreciation to [Appeloper](https://developer.apple.com/forums/profile/Appeloper) for the comprehensive explanation of this architecture in his thread [Stop using MVVM for SwiftUI](https://developer.apple.com/forums/thread/699003).