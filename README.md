# JSON Tools Suite: DataSculptor, Formatter & Converter

## Overview

This application provides a comprehensive suite of JSON processing tools, including:

1. **DataSculptor** - A powerful tool to sculpt your data by precisely filtering and projecting Java objects (POJOs) and JSON data
2. **JSON Formatter** - Beautify and validate your JSON with proper indentation
3. **JSON Converter** - Transform your JSON data to XML or YAML formats

The application combines all these tools in a single interface, allowing you to seamlessly work with JSON data for various purposes - from filtering and projection to formatting and converting to other formats.

## Tools Overview

### JSON Formatter

The JSON Formatter tool allows you to:
- Validate JSON input for syntax errors
- Beautify JSON with proper indentation for improved readability 
- Minify JSON by removing whitespace for reduced file size
- Download the formatted output in JSON, XML, or YAML format

### JSON Converter

The JSON Converter enables you to:
- Convert valid JSON to XML format with proper tag structure
- Convert valid JSON to YAML format with correct indentation
- Visualize the converted output with syntax highlighting
- Download the converted data directly in the target format

## Contributions & Support

Have ideas for improvements or found a bug? We welcome your contributions and feedback!

- **Issue Reporting**: If you encounter any bugs or issues, please report them on our [GitHub Issues page](https://github.com/diogocarleto/jsontools/issues)
- **Feature Requests**: Have an idea for a new feature or enhancement? Submit it as an issue with the "feature request" label
- **Questions**: For general questions about using the tools, open a discussion on GitHub

All contributions, big or small, are appreciated. Visit our repository at [https://github.com/diogocarleto/jsontools](https://github.com/diogocarleto/jsontools) to get involved.

### DataSculptor

DataSculptor is a powerful Java library designed to sculpt your data by precisely filtering and projecting Java objects (POJOs) and JSON data. It allows you to prune nested objects, filter collections based on field values, and create projections that include only specified fields, significantly reducing memory usage and improving performance in data-heavy applications.

The library's name reflects its purpose - like a sculptor, it carefully removes the unnecessary parts to reveal the exact data shape you need.

## Core Features

- **Field Projections**: Extract only the specific fields you need from complex objects
- **Object Filtering**: Filter collections based on field values using a powerful SQL-like DSL
- **Deep Navigation**: Traverse deep into object hierarchies with intuitive dot notation
- **Wildcard Support**: Select all properties at a specific level with `*` notation
- **Support for Various Data Types**: Works with POJOs, JSON strings, Maps, Collections, and Arrays
- **Combined Operations**: Apply both filtering and projections in a single operation
- **In-place Modification**: Modify objects directly for optimal performance
- **Complex Query Support**: Create sophisticated filters with logical operators, parentheses, and comparison operators

## Supported Filter Operators

DataChisel supports a comprehensive set of operators in its filtering DSL for comparing values and creating complex conditions:

### Comparison Operators

| Operator            | Text Form | Symbol Form | Description                                  | Example                                     |
|---------------------|-----------|-------------|----------------------------------------------|---------------------------------------------|
| Equal               | `eq`      | `==`        | Checks if values are equal                   | `age eq 30` or `age == 30`                  |
| Not Equal           | `ne`      | `!=`        | Checks if values are not equal               | `status ne 'INACTIVE'` or `status != 'INACTIVE'` |
| Greater Than        | `gt`      | `>`         | Checks if left value is greater than right   | `price gt 100` or `price > 100`             |
| Less Than           | `lt`      | `<`         | Checks if left value is less than right      | `count lt 5` or `count < 5`                 |
| Greater Than/Equal  | `gte`     | `>=`        | Checks if left value is greater or equal     | `age gte 18` or `age >= 18`                 |
| Less Than/Equal     | `lte`     | `<=`        | Checks if left value is less than or equal   | `quantity lte 10` or `quantity <= 10`       |

### Logical Operators

| Operator | Text Form | Symbol Form | Description                      | Example                                 |
|----------|-----------|-------------|----------------------------------|-----------------------------------------|
| AND      | `and`     | `&&`        | Both conditions must be true     | `age > 20 and status eq 'ACTIVE'`       |
| OR       | `or`      | `\|\|`      | At least one condition must be true | `category eq 'A' or category eq 'B'` |

### Dsl String Filter example

- **Parentheses**: Group conditions to control evaluation order
  - Example: `(age > 30 and gender eq 'M') or name eq 'Mary'`

- **Type Coercion**: Automatic conversion between compatible types
  - Numbers to strings and vice versa
  - Different numeric types (Integer, Long, Double, etc.)

- **Null Handling**: Special handling for null values
  - Only `eq` and `ne` operators work meaningfully with null values
  - Example: `spouse eq 'Mary' ` or `children.name ne 'Bob'`

- **String Comparison**: Supports single quotes
  - Example: `name eq 'John'`

### Advanced Examples

## Using DslFilterHelper for Advanced Filtering

While the string-based DSL filters are convenient for simple use cases, DataChisel provides a more powerful approach using `DslFilterHelper` for complex filtering scenarios. This approach gives you more control over how filters are applied, especially when dealing with nested structures or when you need to apply multiple filters in sequence.

### DslFilterHelper Basics

`DslFilterHelper` is a record class that encapsulates filter information with optional parameters to control filter behavior:

```java
public record DslFilterHelper(
    String filter,                                 // The DSL filter string
    Optional<String> commonRootPrefixOptional,    // Optional root prefix override, the infra will try to detect when not informed
    Optional<Boolean> considerOnlyFirstSegmentOptional // Control segment handling
) { ... }
```

### Creating DslFilterHelper Instances

There are two factory methods for creating instances:

```java
// Basic filter with default options
DslFilterHelper.of("children.age > 10");

// Advanced filter with custom options
DslFilterHelper.of(
    "persons.children.age > 10",           // Filter string
    Optional.of("children"),        // Common root prefix, will prune children, when not informed will try to detect and prune in "persons" when it is the first filter.
    Optional.of(false)              // ignored since commonRootPrefixOptional, it is useful when commonRootPrefixOptional is not informed and the filter is not the first filter, it that scenarios, it will probably detect "children" as root prefix if the filter is not the first filter.
);
```

### Applying Multiple Filters with DslFilterHelper

The `ObjectReducer.applyDslFilters` method accepts a list of `DslFilterHelper` instances, allowing you to apply multiple filters in sequence. This is particularly useful for:

1. Filtering at different levels of a nested object structure
2. Applying progressive filtering where each filter builds on the results of the previous one
3. Fine-tuning how path segments are interpreted for each filter

### Examples

#### Example 1: Multi-level Filtering with Person Hierarchy

This example demonstrates filtering a family structure with multiple levels (parent → children → grandchildren):

```java
// Create a person hierarchy with parent, children, and grandchildren
Person parent = createFamilyHierarchy();

// Apply multiple filters with different nesting levels
ObjectReducer.applyDslFilters(parent, List.of(
    DslFilterHelper.of("children.age < 21"),                // Keep younger children
    DslFilterHelper.of("children.gender == 'female'"),      // Keep only female children
    DslFilterHelper.of("children.children.age < 9")         // Keep only young grandchildren
));

// Result: Only female children under 21 with their grandchildren under 9 remain
```

#### Example 2: Filtering Company Departments and Employees

This example shows how to filter a company structure with departments and employees:

```java
// Create a company with departments and employees
Company company = createCompanyStructure();

// Apply filters to keep senior employees in specific departments
ObjectReducer.applyDslFilters(company, List.of(
    DslFilterHelper.of("departments.name == 'Engineering'"),  // Keep only Engineering department
    DslFilterHelper.of("departments.employees.position eq 'Manager' and departments.employees.age > 40") // Keep senior managers
));

// Result: Only the Engineering department with senior managers remains
```

#### Example 3: Filtering with Null Comparisons

This example demonstrates filtering with null value comparisons:

```java
// Create a person with some children having null gender values
Person person = createPersonWithChildren();

// Apply filter to keep only children with null gender or specific name
ObjectReducer.applyDslFilters(person, List.of(
    DslFilterHelper.of("children.gender == null or children.name == 'Child3'")
));

// Result: Only children with null gender or named 'Child3' remain
```

#### Example 4: Filtering JSON Data

This example shows filtering JSON data with nested structures:

```java
// Parse JSON string into an object structure
Object jsonObject = parseJsonString(jsonData);

// Apply filters to the JSON structure
ObjectReducer.applyDslFilters(jsonObject, List.of(
    DslFilterHelper.of("families.members.age > 40"),          // Keep only members over 40
    DslFilterHelper.of("families.members.children.gender == 'F'") // Keep only female children
));

// Result: JSON structure with only members over 40 and their female children
```

### Benefits of Using DslFilterHelper

- **Sequential Filtering**: Apply filters in a specific order to achieve complex filtering logic
- **Fine-grained Control**: Control how path segments are interpreted for each filter
- **Optimized Performance**: Reduce unnecessary processing by filtering at different levels
- **Improved Readability**: Break complex filtering logic into smaller, more manageable pieces

## Using Projections with ObjectReducerProjectionTreeFilter

In addition to filtering, DataChisel provides powerful projection capabilities through the `ObjectReducerProjectionTreeFilter` class. Projections allow you to specify exactly which fields to include in your objects, effectively pruning all other fields. This is particularly useful for:

1. Reducing memory usage by excluding unnecessary fields
2. Improving serialization performance by reducing object size
3. Creating tailored views of your data for specific use cases
4. Simplifying complex object structures

### ObjectReducerProjectionTreeFilter Basics

The `ObjectReducerProjectionTreeFilter` class builds a tree structure that represents the fields to include in your projection. You can create projection filters using the static `build` method:

```java
// Create a projection filter for specific fields
List<ObjectReducerProjectionTreeFilter> projectionFilters = 
    ObjectReducerProjectionTreeFilter.build(List.of("uuid", "name", "address.city"));
```

### Applying Projections

Use the `ObjectReducer.applyProjections` method to apply your projections to an object:

```java
// Apply projections to include only specified fields
ObjectReducer.applyProjections(entity, projectionFilters);
```

### Examples

#### Example 1: Basic Field Projection

This example demonstrates how to include only specific fields at the root level:

```java
// Create a space object with multiple fields
SpaceResponse space = SpaceResponse.of("space-123", "Meeting Room", 500L);

// Apply projection to keep only the UUID field
ObjectReducer.applyProjections(
    space, 
    ObjectReducerProjectionTreeFilter.build(List.of("uuid"))
);

// Result: space object now only has uuid field populated, other fields are null
// space.uuid = "space-123";
// space.name = null;
// space.size = null;
```

#### Example 2: Nested Field Projection

This example shows how to include fields from nested objects:

```java
// Create a company with departments and employees
Company company = createCompanyStructure();

// Apply projections to include only specific nested fields
ObjectReducer.applyProjections(
    company, 
    ObjectReducerProjectionTreeFilter.build(List.of(
        "name",
        "departments.name",
        "departments.employees.name"
    ))
);

// Result: Only company name, department names, and employee names are included
// All other fields like employee age, salary, etc. are set to null
```

#### Example 3: Wildcard Projection

This example demonstrates using wildcards to include all fields at a specific level:

```java
// Create a space with rooms and products
SpaceResponse space = createSpaceWithRoomsAndProducts();

// Apply projections with wildcards
ObjectReducer.applyProjections(
    space,
    ObjectReducerProjectionTreeFilter.build(List.of(
        "uuid",
        "rooms.*"  // Include all direct fields of rooms
    ))
);

// Result: space.uuid is included, all direct fields of rooms are included
// but nested objects within rooms (like products) retain all their fields
```

#### Example 4: Combining Projections and Filters

This example shows how to combine projections with filters for maximum data sculpting:

```java
// Create a space with rooms and products
SpaceResponse space = createSpaceWithRoomsAndProducts();

// Apply both projections and filters
ObjectReducer.applyProjectionsAndDslFilters(
    space,
    ObjectReducerProjectionTreeFilter.build(List.of(
        "uuid", 
        "rooms.uuid", 
        "rooms.title", 
        "rooms.products.*"  // Include all fields of products
    )),
    List.of(
        DslFilterHelper.of("rooms.uuid eq 'room-123' and rooms.products.price > 1000")
    )
);

// Result: Only rooms with uuid 'room-123' and products with price > 1000 are included
// Only the specified fields are populated, others are null
```

### Benefits of Using Projections

- **Memory Efficiency**: Include only the fields you need, reducing memory usage
- **Performance**: Improve serialization and deserialization performance with smaller objects
- **Bandwidth Optimization**: Reduce network traffic when transferring data
- **Simplified Views**: Create tailored views of complex data structures
- **Targeted Data Access**: Access only the specific data paths you need

### Projection Patterns

- **Root Level Fields**: Simple field names like `"name"` or `"uuid"`
- **Nested Fields**: Dot notation paths like `"department.employees.name"`
- **Wildcards**: Use `"*"` to include all fields at a specific level, like `"rooms.*"`
- **Combined Patterns**: Mix specific fields and wildcards as needed

## Combining Filters and Projections

DataChisel allows you to combine the power of both filtering and projection in a single operation using the method:

```java
ObjectReducer.applyProjectionsAndDslFilters(
    entity,
    List<ObjectReducerProjectionTreeFilter> treeFilterList,
    List<DslFilterHelper> dslFilterHelpers
);
```

This enables you to first filter your data (removing unwanted elements from collections or nested objects) and then project only the fields you care about, resulting in highly optimized and tailored data structures.

### Example 1: Filter and Project Nested Collections

```java
// Create a space with rooms and products
SpaceResponse space = createSpaceWithRoomsAndProducts();

// Define projection: keep only uuid, room details, and all product fields in rooms
List<ObjectReducerProjectionTreeFilter> projections = ObjectReducerProjectionTreeFilter.build(List.of(
    "uuid",
    "rooms.uuid",
    "rooms.title",
    "rooms.products.*"
));

// Define filters: keep only rooms and products matching certain criteria
List<DslFilterHelper> filters = List.of(
    DslFilterHelper.of("rooms.uuid eq 'room-123' and rooms.products.price > 1000")
);

// Apply both filters and projections
ObjectReducer.applyProjectionsAndDslFilters(space, projections, filters);

// Result: Only rooms with uuid 'room-123' and products with price > 1000 are included,
// and only the specified fields are populated (others are set to null)
```

### Example 2: Multi-level Filtering and Projection

```java
// Create a company with departments and employees
Company company = createCompanyStructure();

// Define projections to keep only department and employee names
List<ObjectReducerProjectionTreeFilter> projections = ObjectReducerProjectionTreeFilter.build(List.of(
    "departments.name",
    "departments.employees.name"
));

// Define filters to keep only employees with salary > 90000
List<DslFilterHelper> filters = List.of(
    DslFilterHelper.of("departments.employees.salary > 90000")
);

ObjectReducer.applyProjectionsAndDslFilters(company, projections, filters);

// Result: Only departments and employees matching the filter remain, and only their names are present
```

### Example 3: Wildcards and Filtering

```java
// Create a space with rooms and products
SpaceResponse space = createSpaceWithRoomsAndProducts();

// Projection: keep all fields of rooms, but only uuid of products
List<ObjectReducerProjectionTreeFilter> projections = ObjectReducerProjectionTreeFilter.build(List.of(
    "rooms.*",
    "rooms.products.uuid"
));

// Filter: keep only products with uuid starting with 'prod-'
List<DslFilterHelper> filters = List.of(
    DslFilterHelper.of("rooms.products.uuid =~ 'prod-.*'")
);

ObjectReducer.applyProjectionsAndDslFilters(space, projections, filters);

// Result: All room fields are present, but for products only uuid is included, and only products matching filter remain
```

### Why Combine Filters and Projections?

- **Maximum Efficiency**: Remove unnecessary data and fields in a single pass
- **Tailored Results**: Produce objects optimized for your downstream consumers
- **Cleaner APIs**: Return only what is needed, nothing more
- **Improved Security**: Exclude sensitive or irrelevant fields by default

