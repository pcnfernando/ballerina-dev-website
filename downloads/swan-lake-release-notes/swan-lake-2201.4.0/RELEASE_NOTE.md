---
layout: ballerina-left-nav-release-notes
title: 2201.4.0 (Swan Lake) 
permalink: /downloads/swan-lake-release-notes/2201.4.0/
active: 2201.4.0
redirect_from: 
    - /downloads/swan-lake-release-notes/2201.4.0
    - /downloads/swan-lake-release-notes/2201.4.0/
    - /downloads/swan-lake-release-notes/2201.4.0-swan-lake/
    - /downloads/swan-lake-release-notes/2201.4.0-swan-lake
    - /downloads/swan-lake-release-notes/
    - /downloads/swan-lake-release-notes
---

## Overview of Ballerina Swan Lake 2201.4.0

<em>2201.4.0 (Swan Lake) is the fourth major release of 2022, and it includes a new set of features and significant improvements to the compiler, runtime, standard library, and developer tooling. It is based on the 2022R4 version of the Language Specification.</em> 

## Update Ballerina

**If you are already using Ballerina 2201.0.0 (Swan Lake)**, run either of the commands below to directly update to 2201.4.0 using the [Ballerina Update Tool](/learn/cli-documentation/update-tool/).

`bal dist update` (or `bal dist pull 2201.4.0`)

**If you are using a version below 2201.0.0 (Swan Lake)**, run the commands below to update to 2201.4.0.

1. Run `bal update` to get the latest version of the Update Tool.

2. Run `bal dist update` ( or `bal dist pull 2201.4.0`) to update your Ballerina version to 2201.4.0.

However, if you are using a version below 2201.0.0 (Swan Lake) and if you already ran `bal dist update` (or `bal dist pull 2201.4.0`) before `bal update`, see [Troubleshooting](/downloads/swan-lake-release-notes/swan-lake-2201.0.0#troubleshooting) to recover your installation.

## Install Ballerina

If you have not installed Ballerina, download the [installers](/downloads/#swanlake) to install.

## Language updates

### New features

#### Support for annotations on tuple members

- Annotations are now supported on tuple members. Tuple members can be annotated using annotations declared with the `field` attachment point.

  ```ballerina
  annotation annot on field;

  type T [int, @annot
      string ];
  ```

  Annotations are not allowed on the tuple rest descriptor. 

### Bug fixes

- Annotation values of fields of record type descriptors that are not defined with a type definition are now accessible at runtime.

- To view other bug fixes, see the [GitHub milestone for Swan Lake 2201.4.0](https://github.com/ballerina-platform/ballerina-lang/issues?q=is%3Aissue+label%3ATeam%2FCompilerFE+milestone%3A2201.4.0+is%3Aclosed+label%3AType%2FBug).

## Runtime updates

### New features

#### Get the strand dump during `bal test`

- When running the tests in a Ballerina package or a file using the `bal test` command, the strand dump can be obtained by sending the `SIGTRAP` signal to that process.

### Improvements

#### Get the user-defined type name on Singleton types

- Calling `getName()` on the runtime class `FiniteType` will return the user-defined type name if it is available.

  For example, if a constant is defined in the following way, the `getName()` method on the `FiniteType` will return the string `"OPEN"`.

  ```ballerina
  const OPEN = "open";
  ```

#### Type-reference type support in runtime Java APIs

- The following runtime APIs are now modified to return the type-reference type instances according to the type definitions.

  | **Runtime API**                                                          | **Java class**                                     |
  |--------------------------------------------------------------------------|----------------------------------------------------|
  | `getElementType`                                                         | `io.ballerina.runtime.api.types.ArrayType`         |
  | `getDetailType`                                                          | `io.ballerina.runtime.api.types.ErrorType`         |
  | `getFieldType`                                                           | `io.ballerina.runtime.api.types.Field`             |
  | `getParameterTypes`                                                      | `io.ballerina.runtime.api.types.FunctionType`      |
  | `getReturnType`                                                          | `io.ballerina.runtime.api.types.FunctionType`      |
  | `getReturnParameterType`                                                 | `io.ballerina.runtime.api.types.FunctionType`      |
  | `getRestType`                                                            | `io.ballerina.runtime.api.types.FunctionType`      |
  | `getParameters` (The `Parameter.type` field can be a type-reference type.) | `io.ballerina.runtime.api.types.FunctionType`      |
  | `getConstituentTypes`                                                    | `io.ballerina.runtime.api.types.IntersectionType`  |
  | `getEffectiveType`                                                       | `io.ballerina.runtime.api.types.IntersectionType`  |
  | `getConstrainedType`                                                     | `io.ballerina.runtime.api.types.MapType`           |
  | `getParamValueType`                                                      | `io.ballerina.runtime.api.types.ParameterizedType` |
  | `getRestFieldType`                                                       | `io.ballerina.runtime.api.types.RecordType`        |
  | `getReferredType`                                                        | `io.ballerina.runtime.api.types.ReferenceType`     |
  | `getConstrainedType`                                                     | `io.ballerina.runtime.api.types.StreamType`        |
  | `getCompletionType`                                                      | `io.ballerina.runtime.api.types.StreamType`        |
  | `getConstrainedType`                                                     | `io.ballerina.runtime.api.types.TableType`         |
  | `getTupleTypes`                                                          | `io.ballerina.runtime.api.types.TupleType`         |
  | `getRestType`                                                            | `io.ballerina.runtime.api.types.TupleType`         |
  | `getConstraint`                                                          | `io.ballerina.runtime.api.types.TypedescType`      |
  | `getMemberTypes`                                                         | `io.ballerina.runtime.api.types.UnionType`         |
  | `getOriginalMemberTypes`                                                 | `io.ballerina.runtime.api.types.UnionType`         |
  | `getElementType`                                                         | `io.ballerina.runtime.api.values.BArray`           |
  | `getConstraintType`                                                      | `io.ballerina.runtime.api.values.BStream`          |
  | `getCompletionType`                                                      | `io.ballerina.runtime.api.values.BStream`          |
  | `getKeyType`                                                             | `io.ballerina.runtime.api.values.BTable`           |
  | `getDescribingType`                                                      | `io.ballerina.runtime.api.values.BTypedesc`        |
  | `getType`                                                                | `io.ballerina.runtime.api.values.BValue`           |
  | `getType`                                                                | `io.ballerina.runtime.api.utils.TypeUtils`         |

  For example, if the type-reference types are defined in the following way,

  ```ballerina
  type Integer int;

  type IntegerArray Integer[];

  IntegerArray arr = [1, 2, 3, 4];
  ```
  the results of the runtime API calls will be as follows.

  | **Runtime API call**                            | **Result**                                                       |
  |-------------------------------------------------|------------------------------------------------------------------|
  | `arr.getType()`                                 | This will return a `ReferenceType` with the name `IntegerArray`. |
  | `getReferredType()` on `IntegerArray`           | This will return an `ArrayType` with name `IntegerArray`.        |
  | `getElementType()` on `IntegerArray` array type | This will return a `ReferenceType` with the name `Integer`.      |

  <br>

  > **Note:**
  > The definition of the `getType` API in the `BObject` runtime class is now modified to the following.
  > ```java
  >  Type getType();
  > ```

### Bug fixes

To view bug fixes, see the [GitHub milestone for 2201.4.0 (Swan Lake)](https://github.com/ballerina-platform/ballerina-lang/issues?q=is%3Aissue+milestone%3A2201.4.0+label%3ATeam%2FjBallerina+label%3AType%2FBug+is%3Aclosed).

## Standard library updates

### New features

#### `constraint` package

- Introduced the `@pattern` constraint on string types.
- Added constraint validation support for `readonly` types.

#### `oauth2` package

- Allow the use of inferred values for refreshing tokens through the password grant type.
- Allow the use of string values for the `scopes` field in the client grant configuration. 

#### `graphql` package

- Added support for multiplexing in GraphQL subscriptions.
- Added support to access the GraphQL field information from resolvers.

#### `nats` package

- Added support for the new NATS JetStream client with publishing and subscribing functionalities.

#### `persist` package

- Added support for Ballerina Persistent Layer. This functionality is provided with the newly introduced `persist` package.

  The Ballerina persistent layer provides the functionality of storing and querying data conveniently.
  >**Info:** This is an experimental feature. APIs might change in future releases.

### Improvements

#### `graphql` package

- Removed the limitation on the GraphQL `context` object parameter order.

### Backward-incompatible changes

#### `io` package

- Made the column headers mandatory in CSV files to ensure the order of fields while mapping CSV files to records. This is only applicable for the case where the expected type is `record[]`.
- Made the column headers automatically be written to the CSV file to ensure the order of fields while writing a `record[]` to a CSV.

### Bug fixes

To view bug fixes, see the [GitHub milestone for Swan Lake 2201.4.0](https://github.com/ballerina-platform/ballerina-standard-library/issues?q=is%3Aclosed+is%3Aissue+milestone%3A%222201.4.0%22+label%3AType%2FBug).

### Code to Cloud updates

### New features
- Added the ability to disable the thin JAR file generation by setting `settings.thinJar = false` in `Cloud.toml`.

### Improvements
- Improved the main class name generation in `CMD` to support complex package names.

### Bug fixes

To view bug fixes, see the [GitHub milestone for 2201.4.0 (Swan Lake)](https://github.com/ballerina-platform/module-ballerina-c2c/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22Ballerina+2201.4.0%22+label%3AType%2FBug).

## Developer tools updates

### New features

#### Test Framework

- Added support for executing tests using a GraalVM native image (experimental).
  
  >**Info:** Introduced the `--native` flag to `bal test` command, which executes tests using a GraalVM native executable.

#### Language Server

- Added the `Create variable with Type` code action.
- Added rename popup support for `Extract to constant`, `Extract to local variable` , and `Extract to function` code actions.
 >**Info:** This feature will be available from the [Ballerina VSCode plugin](https://marketplace.visualstudio.com/items?itemName=WSO2.ballerina) version `4.0.0`.
- Added quick pick support for selecting expressions in the `Extract to constant` code action.

#### GraphQL Tool

- Added support for GraphQL SDL schema file generation.

#### OpenAPI Tool

- Added support to generate Ballerina client and service declarations from Swagger 2.0 (i.e., OpenAPI 2.0) definitions.

#### Persist Tool

- Added a new `persist` command for users to enable the Ballerina persistence layer in a Ballerina project conveniently.

  With this support, users can define an entity data model, validate the model, and generate `persist` clients. This provides convenient APIs to store and query data in a data store.

  >**Info:** This is an experimental feature; the commands associated with the tool might change in future releases.

### Improvements

#### CLI

- Improved the `bal format` CLI command by introducing new command options to selectively format modules or single files with a provided Ballerina package. The updated format of the `bal format` command is shown below.

    ```bash
    bal format [OPTIONS] [<package>|<module>|<source-file>]
    ```

    ```
    OPTIONS
        --module <module-name>
            Format only a specific module in the Package
    
         --file <file-name>
            Format only a specific file in the Package
        
         -d, --dry-run
            Perform a dry run of the formatter and see which files will
            be formatted after the execution.
    ```

#### JSON-to-record converter

- Improved the JSON-to-record converter tool to be more context-aware and generate records with non-conflicting names.

#### Language Server

- Improved the completion and code action support for pulled modules.
- Improved sorting of module-level completion items.
- Used the code action resolve request for compiler plugin code actions.

#### OpenAPI Tool

- Added support for the `additionalProperties` attribute in OpenAPI object schemas. With this support, the generated Ballerina records can be either open or closed records, based on the `additionalProperties` details.
  >**Info:** Thereby, some of your already generated open records may change into closed records, when re-generating using 2201.4.0 (and above) versions.
- Improved support for `nullable:true` property OpenAPI schema, to generate record fields with default values (e.g. `string? name = ();`), instead of making the field both nilable and optional (e.g. `string? name?;`).
- Changed the default request and response types of the generated Ballerina resource/remote methods from `json` to `http:Request` and `http:Response`, respectively.


### Breaking changes

- New improvements that were added to the `bal format` command to address some of the existing [limitations](https://github.com/ballerina-platform/ballerina-lang/issues/37868) may break the CLI usages of the `bal format <module-name>` option. 

  >**Info:** In such instances, the `bal format <package-path> --module <module-name>` option can be used for the same purpose from the Swan Lake Update 4 release onwards.

### Bug fixes

To view bug fixes, see the GitHub milestone for Swan Lake 2201.4.0 of the repositories below.

- [Language Server](https://github.com/ballerina-platform/ballerina-lang/issues?q=is%3Aissue+label%3ATeam%2FLanguageServer+milestone%3A2201.4.0+is%3Aclosed)
- [Debugger](https://github.com/ballerina-platform/ballerina-lang/issues?q=is%3Aissue+milestone%3A2201.4.0+is%3Aclosed+label%3AArea%2FDebugger)
- [OpenAPI Tool](https://github.com/ballerina-platform/openapi-tools/issues?q=is%3Aclosed+milestone%3A%22Swan+Lake+2201.4.0+%22+label%3AType%2FBug)

## Ballerina packages updates

### New features

- Added support for maintaining generated code in a Ballerina package.