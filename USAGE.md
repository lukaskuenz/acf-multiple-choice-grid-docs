# Usage example (generic)

## Example field configuration

### Rows
```
option_a : Option A
option_b : Option B
option_c : Option C
```

### Columns
```
yes : Yes
no : No
maybe : Maybe
```

### Notes on definitions

- Rows and columns support:
  - `value : Label`
  - `Label` (value auto-generated via `sanitize_title`)
- Stored values are always `row_value → column_value`
- Labels can be changed later without breaking existing data
- If enabled, the optional **No answer** column uses a configurable label

---

## PHP template usage
### 1) Return Value: Value

Return Shape: **Row → Column map**

```php
$choices = get_field('example_choice_grid');

/*
Result:
[
  'option_a' => 'yes',
  'option_b' => 'no',
  'option_c' => 'maybe',
]
*/

if (is_array($choices)) {
  foreach ($choices as $rowValue => $columnValue) {
    echo esc_html($rowValue . ': ' . $columnValue) . '<br>';
  }
}
```

### 2) Return Value: Label

Return Shape: **Row → Column map**

```php
$choices = get_field('example_choice_grid');

/*
Result:
[
  'Option A' => 'Yes',
  'Option B' => 'No',
  'Option C' => 'Maybe',
]
*/

if (is_array($choices)) {
  foreach ($choices as $rowLabel => $columnLabel) {
    echo esc_html($rowLabel . ': ' . $columnLabel) . '<br>';
  }
}
```

### 3) Return Value: Both (Array)

Return Shape: Row → Column map (structured output)

```php
$choices = get_field('example_choice_grid');

/*
Result:
[
  [
    'row' => [
      'value' => 'option_a',
      'label' => 'Option A',
    ],
    'column' => [
      'value' => 'yes',
      'label' => 'Yes',
    ],
  ],
]
*/

if (is_array($choices)) {
  foreach ($choices as $item) {
    $rowValue   = $item['row']['value'] ?? '';
    $rowLabel   = $item['row']['label'] ?? '';
    $columnValue = $item['column']['value'] ?? '';
    $columnLabel = $item['column']['label'] ?? '';

    echo esc_html("$rowLabel ($rowValue): $columnLabel ($columnValue)") . '<br>';
  }
}
```

### 4) Return Shape: Pair strings (`row:column`)
```php
$choices = get_field('example_choice_grid');

/*
Result:
[
  'option_a:yes',
  'option_b:no',
  'option_c:maybe',
]
*/

if (is_array($choices)) {
  foreach ($choices as $pair) {
    [$row, $column] = array_pad(explode(':', $pair, 2), 2, '');
    echo esc_html($row . ' → ' . $column) . '<br>';
  }
}
```

### Frontend forms (`acf_form()`)

The field works automatically with frontend ACF forms:

```php
acf_form([
  'post_id' => 'new_post',
  'field_groups' => [123], // your field group ID
  'submit_value' => 'Submit',
]);
```

No additional handling is required.

## Notes
- The stored value is always an associative map of **`row_value → column_value`**
- Changing the return format does not affect stored data
- Labels can be changed later without breaking existing entries
- No frontend styles are applied by default
