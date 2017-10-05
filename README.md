# Sourcery Templates ✨
[Sourcery](https://github.com/krzysztofzablocki/Sourcery) stencil files for generating Vapor 2 boilerplate.

# Models
This collection of templates is related to models and automating their conversions to common Vapor types.

## Model
Automatically generates an initializer and an enum for MySQL enum types.

Example:
```swift
final class User: Model {
	var name: String
	var age: Int?
}
```

Becomes:
```swift
// sourcery: model
final class User: Model {
	var name: String
	var age: Int?

// sourcery:inline:auto:User.Models
    let storage = Storage()


    internal init(
        name: String,
        age: Int? = nil
    ) {
        self.name = name
        self.age = age
    }
// sourcery:end
}
```

#### Annotations

|  Key        | Description                                                            |
| ----------- | -----------------------------------------------------------------------|
|  `enumName` | Generate a Swift enum for MySQL with accessors for a list of all cases.|
|  `enumType` | Set the Swift enum type.                                               |
|  `enumCase` | Create a case.                                                         |
|  `ignore`   | Prevents the property from being included in the generated code.       |

## Preparation
Generates a list of database keys, automates `prepare` and `revert` functions.

Example:
```swift
final class User: Model {
	var name: String
	var age: Int?
}
```

Becomes:
```swift
import Vapor
import Fluent

extension User: Preparation {
	internal enum DatabaseKeys {
		static let id = User.idKey
		static let name = "name"
		static let age = "age"
	}

	// MARK: - Preparations (User)
	internal static func prepare(_ database: Database) throws {
		try database.create(self) {
			$0.id()
			$0.string(DatabaseKeys.name)
			$0.int(DatabaseKeys.age, optional: true)
		}

	}

	internal static func revert(_ database: Database) throws {
		try database.delete(self)
	}
}

```

#### Annotations

| Key                 | Description                                                                                                                       |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------|
| `databaseKey`       | Set the database key (default is the name of the member).                                                                         |
| `preparation`       | Set the database preparation type for the given member. For example `preparation = string` will generate `$0.string(...)`         |
| `enumName`          | Generate a Swift enum for MySQL with accessors for a list of all cases.                                                           |
| `enumType`          | Set the Swift enum type.                                                                                                          |
| `enumCase`          | Create a case.                                                                                                                    |
| `unique`            | Whether or not the field is unique.                                                                                               |
| `foreignTable`      | The table to use while configuring foreign ids. This field is only valid if `preparation` is set to `foreignId`.                  |
| `foreignIdKey`      | The foreign key to use while configuring foreign ids.                                                                             |
| `foreignKeyName`    | The foreign key's name. _note_:  this annotation should only be used when you run into Fluent issues with the auto-generated names|
| `ignore`            | Prevents the preparation from being included in the generated code.                                                               |
| `ignorePreparation` | Prevents the preparation from being included in the generated `Preparation` code.                                                 |

## RowConvertible
Automates `init (row: Row)` and `makeRow` boilerplate.

Example:
```swift
final class User: Model {
	var name: String
	var age: Int?
}
```

Becomes:
```swift
import Vapor
import Fluent

extension User: RowConvertible {
	// MARK: - RowConvertible (User)
	convenience internal init (row: Row) throws {
		try self.init(
			name: row.get(DatabaseKeys.name),
			age: row.get(DatabaseKeys.age)
		)
	}

	internal func makeRow() throws -> Row {
		var row = Row()

		try row.set(DatabaseKeys.name, name)
		try row.set(DatabaseKeys.age, age)

		return row
	}
}
```

#### Annotations

| Key                    | Description                                                                       |
| ---------------------- | --------------------------------------------------------------------------------- |
| `databaseKey`          | Set the database key (default is the name of the member).                         |
| `ignore`               | Prevents the property from being included in the generated code.                  |
| `ignoreRowConvertible` | Prevents the property from being included in the generated `RowConvertible` code. |

## NodeRepresentable
Generates a list of node keys and `makeNode(in context: Context?)`.

Example:
```swift
final class User: Model {
	var name: String
	var age: Int?
}
```

Becomes:
```swift
import Vapor
import Fluent

extension User: NodeRepresentable {
	internal enum NodeKeys: String {
		case name
		case age
	}

	// MARK: - NodeRepresentable (User)
	func makeNode(in context: Context?) throws -> Node {
		var node = Node([:])

		try node.set(User.idKey, id)
		try node.set(NodeKeys.name.rawValue, name)
		try node.set(NodeKeys.age.rawValue, age)

		return node
	}
}
```

#### Annotations

| Key                       | Description                                                                         |
| ------------------------- | ------------------------------------------------------------------------------------|
| `nodeKey`                 | Set the key for node (de)serialization.                                             |
| `ignore`                  | Prevents the property from being included in the generated code.                    |
| `ignoreNodeRepresentable` | Prevents the property from being included in the generated `NodeRepresentable` code.|

## JSONConvertible
Generates a list of JSON keys, `init(json: JSON)` and `makeJSON`.

Example:
```swift
final class User: Model {
	var name: String
	var age: Int?
}
```

Becomes:
```swift
extension User: JSONConvertible {
    internal enum JSONKeys: String {
        case name
        case age
    }

    // MARK: - JSONConvertible (User)
    internal convenience init(json: JSON) throws {
        try self.init(
            name: json.get(JSONKeys.name.rawValue),
            age: json.get(JSONKeys.age.rawValue)
        )
    }

    internal func makeJSON() throws -> JSON {
        var json = JSON()

        try json.set(User.idKey, id)
        try json.set(JSONKeys.name.rawValue, name)
        try json.set(JSONKeys.age.rawValue, age)

        return json
    }
}

extension User: ResponseRepresentable {}
```

#### Annotations

| Key                     | Description                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------|
| `jsonValue`             | Set the value for JSON serialization.                                             |
| `ignore`                | Prevents the property from being included in the generated code.                  |
| `ignoreJSONConvertible` | Prevents the property from being included in the generated `JSONConvertible` code.|

# Tests
These templates are related to unit testing with XCTest.

## LinuxMain
Generates a static `allTests` for every `XCTestCase` and registers them in the `LinuxMain.swift`.

#### Annotations

| Key                   | Description                                                       |
| ----------------------| ----------------------------------------------------------------- |
| `excludeFromLinuxMain`| Prevents the test case from being included in the generated code. |

# Controllers
These templates are for controllers and route collections.

## Route collection

#### Annotations

| Key         | Description |
| ------------| ----------- |
| `controller`|             |

## 🏆 Credits

This package is developed and maintained by the Vapor team at [Nodes](https://www.nodesagency.com).
The package owner for this project is [Steffen](https://github.com/steffendsommer).


## 📄 License

This package is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)