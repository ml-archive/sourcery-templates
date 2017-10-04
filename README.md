# Sourcery Templates ✨
[Sourcery](https://github.com/krzysztofzablocki/Sourcery) stencil files for generating Vapor 2 boilerplate.

# Models
This collection of templates is related to models and automating their conversions to common Vapor types.

## Model
|  Key      | Description                                                     |
| --------- | ----------------------------------------------------------------|
|  enumName | Generate a Swift enum for MySQL with accessors for allCases.    |
|  enumType | Set the Swift enum type.                                        |
|  enumCase | Create a case.                                                  |
|  ignore   | Prevents the property from being included in the generated code.|

## Preparation
| Key               | Description                                                                                                         |
| ----------------- | --------------------------------------------------------------------------------------------------------------------|
| databaseKey       | Set the database key (default is the name of the member).                                                           |
| preparation       | Set the database preparation for the given member. For example `preparation = string` will generate `$0.string(...)`|
| enumName          | Generate a Swift enum for MySQL with accessors for allCases.                                                        |
| enumType          | Set the Swift enum type.                                                                                            |
| enumCase          | Create a case.                                                                                                      |
| unique            | Whether or not the field is unique.                                                                                 |
| foreignTable      | The table to use while configuring foreign ids. This field is only valid if `preparation` is set to `foreignId`.    |
| foreignIdKey      | The foreign key to use while configuring foreign ids.                                                               |
| foreignKeyName    | The foreign key's name.                                                                                             |
| ignore            | Prevents the preparation from being included in the generated code.                                                 |
| ignorePreparation | Prevents the preparation from being included in the generated preparation code.                                     |

## RowConvertible
| Key                  | Description                                                      |
| -------------------- | ---------------------------------------------------------------- |
| databaseKey          | Set the database key (default is the name of the member).        |
| ignore               | Prevents the property from being included in the generated code. |
| ignoreRowConvertible | Prevents the property from being included in the generated code. |

## NodeRepresentable
| Key                    | Description                                                     |
| -----------------------| ----------------------------------------------------------------|
| nodeKey                | Set the key for node (de)serialization.                         |
| ignore                 | Prevents the property from being included in the generated code.|
| ignoreNodeRepresentable| Prevents the property from being included in the generated code.|

## JSONConvertible
| Key                   | Description                                                     |
| --------------------- | ----------------------------------------------------------------|
| jsonKey               | Set the key for JSON (de)serialization.                         |
| jsonValue             | Set the value for JSON serialization.                           |
| ignore                | Prevents the property from being included in the generated code.|
| ignoreJSONConvertible | Prevents the property from being included in the generated code.|

## Property Annotations

| Key   | Description                                                      |
| ------| ---------------------------------------------------------------- |
| ignore| Prevents the property from being included in the generated code. |