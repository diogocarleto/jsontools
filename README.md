# JSON Tools Suite

![JSON Tools Logo](https://github.com/diogocarleto/jsontools/raw/main/logo.png)

A comprehensive suite of JSON processing tools, including DataSculptor for filtering and projection, JSON Formatter for validation and formatting, and JSON Converter for transforming JSON to XML and YAML.

## Table of Contents

- [Overview](#overview)
- [DataSculptor](#datasculptor)
  - [Core Features](#core-features)
  - [Filter Operators](#filter-operators)
  - [Filter Examples](#filter-examples)
  - [Using Projections](#using-projections)
  - [Combining Filters & Projections](#combining-filters--projections)
- [JSON Formatter](#json-formatter)
  - [Key Features](#json-formatter-features)
  - [How to Use](#how-to-use-the-json-formatter)
  - [Pro Tips](#json-formatter-pro-tips)
- [JSON Converter](#json-converter)
  - [Key Features](#json-converter-features)
  - [How to Use](#how-to-use-the-json-converter)
  - [Pro Tips](#json-converter-pro-tips)
- [JSON Rewriter](#json-rewriter)
  - [Key Features](#json-rewriter-features)
  - [How to Use](#how-to-use-the-json-rewriter)
  - [Pro Tips](#json-rewriter-pro-tips)
- [Try It Yourself](#try-it-yourself)
- [Contribute & Support](#contribute--support)

## Overview

JSON Tools is a powerful web-based application that offers multiple tools for working with JSON data:

1. **DataSculptor** - Precisely filter and project Java objects (POJOs) and JSON data
2. **JSON Formatter** - Beautify and validate your JSON with proper indentation
3. **JSON Converter** - Transform your JSON data to XML or YAML formats

The application combines all these tools in a single interface, allowing you to seamlessly work with JSON data for various purposes - from filtering and projection to formatting and converting to other formats.

## DataSculptor

DataSculptor is a powerful tool designed to sculpt your data by precisely filtering and projecting Java objects and JSON data. It allows you to prune nested objects, filter collections based on field values, and create projections that include only specified fields, significantly reducing memory usage and improving performance in data-heavy applications.

The name reflects its purpose - like a sculptor, it carefully removes the unnecessary parts to reveal the exact data shape you need.

### Core Features

- **Field Projections**: Extract only the specific fields you need from complex objects
- **Object Filtering**: Filter collections based on field values using a powerful SQL-like DSL
- **Deep Navigation**: Traverse deep into object hierarchies with intuitive dot notation
- **Wildcard Support**: Select all properties at a specific level with `*` notation
- **Support for Various Data Types**: Works with POJOs, JSON strings, Maps, Collections, and Arrays
- **Combined Operations**: Apply both filtering and projections in a single operation
- **In-place Modification**: Modify objects directly for optimal performance
- **Complex Query Support**: Create sophisticated filters with logical operators, parentheses, and comparison operators

### Filter Operators

DataSculptor supports a comprehensive set of operators in its filtering DSL for comparing values and creating complex conditions:

#### Comparison Operators

| Operator            | Text Form | Symbol Form | Description                                  | Example                                     |
|---------------------|-----------|-------------|----------------------------------------------|---------------------------------------------|
| Equal               | `eq`      | `==`        | Checks if values are equal                   | `age eq 30` or `age == 30`                  |
| Not Equal           | `ne`      | `!=`        | Checks if values are not equal               | `status ne 'INACTIVE'` or `status != 'INACTIVE'` |
| Greater Than        | `gt`      | `>`         | Checks if left value is greater than right   | `price gt 100` or `price > 100`             |
| Less Than           | `lt`      | `<`         | Checks if left value is less than right      | `count lt 5` or `count < 5`                 |
| Greater Than/Equal  | `gte`     | `>=`        | Checks if left value is greater or equal     | `age gte 18` or `age >= 18`                 |
| Less Than/Equal     | `lte`     | `<=`        | Checks if left value is less than or equal   | `quantity lte 10` or `quantity <= 10`       |

#### Logical Operators

| Operator | Text Form | Symbol Form | Description                      | Example                                 |
|----------|-----------|-------------|----------------------------------|-----------------------------------------|
| AND      | `and`     | `&&`        | Both conditions must be true     | `age > 20 and status eq 'ACTIVE'`       |
| OR       | `or`      | `\|\|`      | At least one condition must be true | `category eq 'A' or category eq 'B'` |

### Filter Examples

#### Basic Filters
- Simple equality: `persons.age == 30`
- String comparison: `persons.name eq 'John'`
- Numeric comparison: `persons.children.age > 5`
- Null check: `persons.spouse eq null`

#### Complex Filters
- Logical operators: `persons.age > 30 and persons.gender eq 'M'`
- Parentheses for grouping: `(persons.age > 30 and persons.gender eq 'M') or persons.name eq 'Mary'`
- Nested properties: `persons.children.age > 10 and persons.children.gender eq 'F'`

#### Important Notes
- Use parentheses to control evaluation order
- String values must be enclosed in single quotes, e.g., `name eq 'John'`
- Only `eq` and `ne` operators work meaningfully with null values
- Use dot notation to access nested properties

### Using Projections

Projections allow you to specify exactly which fields to include in your objects, effectively pruning all other fields.

#### Projection Examples
- Root level field: `uuid`
- Nested field: `persons.name`
- Multiple nested fields: `persons.name`, `persons.age`, `persons.gender`
- Deep nesting: `persons.children.name`
- All fields at a level: `persons.*` (wildcard)

#### Benefits of Using Projections
- Memory Efficiency: Include only the fields you need, reducing memory usage
- Performance: Improve serialization and deserialization performance with smaller objects
- Bandwidth Optimization: Reduce network traffic when transferring data
- Simplified Views: Create tailored views of complex data structures

### Combining Filters & Projections

DataSculptor allows you to combine the power of both filtering and projection in a single operation for highly optimized and tailored data structures.

#### Example 1: Filter persons by age and project only names
- Filter: `persons.age > 30`
- Projections: `persons.name`, `persons.age`
- Result: Only persons over 30 years old with just their name and age fields

#### Example 2: Filter by nested properties and project specific fields
- Filter: `persons.children.gender eq 'F' and persons.children.age < 10`
- Projections: `persons.name`, `persons.children.name`, `persons.children.age`
- Result: Only persons with female children under 10, including only the specified fields

## JSON Formatter

The JSON Formatter is a simple but powerful tool that helps you validate, beautify, and work with JSON data effectively.

### JSON Formatter Features

- **Validate JSON**: Instantly check if your JSON is valid and well-formatted with detailed error reporting
- **Beautify JSON**: Transform minified JSON into properly indented, readable format
- **View JSON**: Browse complex JSON with the integrated JSON viewer
- **Save and Share**: Download formatted JSON or copy to clipboard with a single click

### How to Use the JSON Formatter

1. Select the "JSON Formatter" tab in the main playground
2. Enter or paste your JSON data into the input field
3. Alternatively, upload a JSON file or enter a URL to fetch remote JSON
4. Click "Validate JSON" to check for syntax errors with detailed feedback
5. Click "Format JSON" to beautify your valid JSON
6. View the formatted result in the output panel
7. Use the download or copy buttons to save or share your formatted JSON

### JSON Formatter Pro Tips

- **Large JSON Files**: The formatter can handle large JSON files up to 8MB in size
- **Remote JSON**: Load JSON directly from a URL by entering the web address in the URL field
- **Expanded View**: Use the expand button for a larger view of complex JSON structures
- **Error Highlighting**: Quickly identify syntax errors with helpful highlighting and error messages
- **Detailed Error Reporting**: When validation fails, get specific information about line and column numbers where errors occur

## JSON Converter

The JSON Converter is a versatile independent tool that allows you to convert from JSON to other popular data formats including XML and YAML.

### JSON Converter Features

- **One-way Conversion**: Convert from JSON to XML or YAML formats
- **Multiple Format Support**: Convert JSON to XML and YAML target formats
- **Pretty Formatting**: Output is automatically formatted for readability
- **Download Options**: Save converted data in your preferred format

### How to Use the JSON Converter

1. Select the "JSON Converter" tab in the main playground
2. Enter or paste your JSON data into the input field, or upload a JSON file
3. Select the target format you want to convert to (XML or YAML) from the dropdown
4. Click "Convert" to transform your data
5. View the converted result in the output panel
6. Use the download button to save the result in your selected format

### JSON Converter Pro Tips

- **XML Attributes**: When converting from JSON to XML, nested objects can be represented as either nested elements or attributes
- **YAML Indentation**: The converter handles YAML's strict indentation requirements automatically
- **JSON Validation**: Your JSON input is automatically validated before conversion
- **Integration**: Use the converter in conjunction with DataSculptor to filter data and then convert it to your preferred format

## JSON Rewriter

The JSON Rewriter is a powerful tool that allows you to modify specific fields in your JSON data using dot notation paths, making it easy to transform and update JSON structures without complex programming.

### JSON Rewriter Features

- **Field Modification**: Update specific fields in your JSON using dot notation paths
- **Multiple Rewrites**: Apply multiple field modifications in a single operation
- **Nested Field Support**: Modify deeply nested fields with intuitive path notation
- **Value Validation**: Ensure new values are valid JSON before applying changes
- **Preview Changes**: See the impact of your changes before applying them
- **Batch Processing**: Apply the same modifications to multiple JSON documents

### How to Use the JSON Rewriter

1. Select the "JSON Rewriter" tab in the main playground
2. Enter or paste your JSON data into the input field
3. Add one or more rewrite rules:
   - Enter the dot notation path for the field to modify
   - Provide the new value in valid JSON format
4. Click "Apply Changes" to modify your JSON
5. View the modified result in the output panel
6. Use the download or copy buttons to save or share your modified JSON

### JSON Rewriter Pro Tips

- **Path Syntax**: Use dot notation for nested fields (e.g., `user.address.city`)
- **Array Elements**: Access array elements using index notation (e.g., `users.0.name`)
- **Wildcards**: Use `*` to match all elements in an array or object
- **Complex Paths**: Combine multiple path expressions for sophisticated modifications
- **Value Types**: Ensure new values match the expected type of the target field
- **Validation**: Always validate your JSON after making modifications

## Try It Yourself

Ready to start using these powerful JSON tools? Visit [https://jsontools.deckend.io](https://jsontools.deckend.io) to try out:

- **DataSculptor**: Try different filter and projection combinations on your JSON data
- **JSON Formatter**: Format and validate your JSON data
- **JSON Converter**: Convert between JSON, XML, YAML, and CSV formats
- **JSON Rewriter**: Modify specific fields in your JSON using dot notation paths

Each tool features a clean, intuitive interface and real-time preview of your changes.

## Contribute & Support

We welcome your feedback, bug reports, and feature requests to help improve JSON Tools!

### How to Contribute

- **Report Issues**: Found a bug? Let us know by [creating an issue](https://github.com/diogocarleto/jsontools/issues) on GitHub
- **Feature Requests**: Have an idea for a new feature? Submit it as an issue with the "feature request" label on our [GitHub repository](https://github.com/diogocarleto/jsontools)
- **Ask Questions**: For general questions, start a discussion on our GitHub repository

All contributions, big or small, are appreciated. Visit our repository at [https://github.com/diogocarleto/jsontools](https://github.com/diogocarleto/jsontools) to get involved.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

&copy; 2025 Deckend. JSON Tools Playground - Powerful JSON filtering, projection, formatting, and conversion capabilities. 
