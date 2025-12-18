# ACF Multiple Choice Grid

Google-Forms-style multiple choice grids for Advanced Custom Fields.

ACF Multiple Choice Grid adds a custom ACF field type that allows editors to select **exactly one option per row** in a structured row/column grid - similar to the "Multiple Choice Grid" field in Google Forms.

It is designed for developers and agencies building structured submissions, applications, evaluations, and internal tools with ACF.

> This repository contains documentation and usage examples for  
> **ACF Multiple Choice Grid**, a custom field type for Advanced Custom Fields.
>
> The plugin itself is distributed separately.

---

## Why this plugin exists

Advanced Custom Fields provides excellent choice fields (Radio, Checkbox, Select), but it does not support matrix or grid-style questions out of the box.

ACF Multiple Choice Grid fills this gap with a **small, focused, predictable field type** that integrates seamlessly into existing ACF workflows.

No form builder.  
No magic.  
Just a clean ACF field that does one thing well.

---

## Features

- Multiple choice grid (radio-style, one selection per row)
- Rows and columns defined using familiar `value : label` syntax
- Supports label-only definitions (values are auto-generated)
- Fully compatible with `acf_form()` and frontend submissions
- ACF-style return formats:
  - **Value**
  - **Label**
  - **Both (Array)**
- Flexible return shapes:
  - Columns only
  - Row &#8594; Column map
  - Pair strings (e.g. `row:column`)
- Optional "No answer" column with configurable label
- Semantic, accessible table markup
- No frontend styling assumptions

---

## Typical use cases

- Application and submission forms
- Jury or evaluation systems
- Grant or funding portals
- Internal admin tools
- Any scenario where multiple items need the same structured choice

---

## Requirements

- **Advanced Custom Fields Pro**
- WordPress 6.x or newer
- PHP 8.0 or newer

---

## Usage

1. Install and activate the plugin
2. Create a new ACF field of type **Multiple Choice Grid**
3. Define rows and columns using one entry per line
4. Choose how values should be returned (Value / Label / Both)
5. Choose how rows and columns should be mapped
6. Use `get_field()` as usual in your templates

### Example

Use it to keep track of item availabilty.

#### Example: Rows ([value] : [label])

- item_a : Item A
- item_b : Item B
- item_c : Item C


#### Example: Columns ([value] : [label])

- available : Available
- limited : Limited
- unavailable : Not available
 
### Notes on row / column definitions

- Definitions follow ACF-style “Choices” syntax:
  - `value : Label` (recommended, stable)
  - `Label` (value will be auto-generated using `sanitize_title`)
- If **Allow no answer** is enabled, an additional column is shown.
  - Its label can be customized via the **No answer label** field setting.

---

## Example output - Column only

Depending on the field settings, `get_field()` may return:

### Value
```php
[
  'available',
  'unavailable',
]
```

### Label
```php
[
  'Available',
  'Not available',
]
```

### Both (Array)
```php
[
  ['value' => 'limited', 'label' => 'Limited'],
]
```

## Example output - Row &#8594; Column mapping

### Value
```php
[
  'item_a' => 'available',
  'item_b' => 'unavailable',
]
```

### Label
```php
[
  'Item A' => 'Available',
  'Item B' => 'Not available',
]
```

### Both (Array)
```php
[
  [
    'row' => ['value' => 'item_a', 'label' => 'Item A'],
    'column' => ['value' => 'limited', 'label' => 'Limited'],
  ],
]
```

## Example output – Pair strings
### Pair strings (`row_value:column_value`)
```php
[
  'item_a:available',
  'item_b:unavailable',
]
```

### Pair string return format (important)
When using Return Shape: Pair strings, the field will always return values in the format:
`row_value:column_value`

This applies regardless of the selected Return Value (Value, Label, or Both).

The pair string format is designed to be:
- Stable
- Machine-readable
- Easy to parse in PHP, JavaScript, or external systems

For human-readable output, use Row → Column map with Label or Both (Array) instead.

## Limitations

- Exactly **one option per row**
- No scoring, weighting, or analytics logic
- No frontend styles included (by design)

## Support

This plugin is provided on a **best-effort basis**.
- Bug reports and suggestions via GitHub Issues
- No guaranteed response times
- No SLA or priority support

## License

Commercial license.
- One-time purchase
- Lifetime updates
- Commercial use allowed
- No subscriptions

&copy; [Lukas K&uuml;nz](https://lukaskuenz.de)
