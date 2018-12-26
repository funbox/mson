# Array of fixed size

The information below describes all possible options to define fixed-size array in various forms.

**fixed-size array**  - array, which contains a finite number of elements of any values and/or types, and this number is strictly defined.

**fixed array** - array, which contains a finite number of elements of only the specified values and/or types, and this number is strictly defined.

### Array size restriction

_Size Restriction_ →  _Size Range_

_Size Range_ → `min-length="`*Value*`"` | `max-length="`*Value*`"` | `min-length="`*Value*`"` , `max-length="`*Value*`"`

_Size Range_  provides the information about minimum and maximum number of elements, contained in an instance of type.
`Value` MUST be either a positive integer or zero. A value of `min-length` attribute MUST be less or equal to a value of `max-length` attribute.

### Array type definition

_Type Definition_ → `(`_Type Specification_ `,` _[opt]_ _Type Attributes List_ _[opt]_` ,` _[opt]_ _Size Restriction_ _[opt]_`)`


## Fixed-size array description variants

### 1. Inline MSON value member (no nested member types)

**MSON**

```mson
+ colors (array[string], min-length="3", max-length="3")
```
implies an array, which MUST contain exactly 3 elements, and each element SHOULD be of fixed type `string`.

**JSON Schema**

```json
{
  "colors": {
    "type": "array",
    "minItems": 3,
    "maxItems": 3,
    "items": {
      "type": "string"
    }
  }
}
```

**MSON**

```mson
+ colors (array[string, number], min-length="3", max-length="3")
```
implies an array, which MUST contain exactly 3 elements, and each element SHOULD be either `string` or `number`.

**JSON Schema**

```json
{
  "colors": {
    "type": "array",
    "minItems": 3,
    "maxItems": 3,
    "items": {
      "anyOf": [
        {
          "type": "string"
        },
        {
          "type": "number"
        }
      ]
    }
  }
}
```

**MSON**

```mson
+ location (array[Point], min-length="2", max-length="2")
```
implies an array, which MUST contain exactly 2 items, and each item should represent an element of user-defined _Named Type_ `Point`.

Where `Point` may have the next structure (as an example):

```mson
# Point (object)

+ x (number) - coordinate on x-axis
+ y (number) - coordinate on y-axis
```

**JSON Schema**

```json
{
  "location": {
    "type": "array",
    "minItems": 2,
    "maxItems": 2,
    "items": {
      "type": "object",
      "properties": {
        "x": {
          "type": "number"
        },
        "y": {
          "type": "number"
        }
      },
      "additionalProperties": false
    }
  }
}
```

**MSON**

```mson
+ colors (array[string], fixed)
```

implies an array, which MUST contain the only element of fixed type `string`

**JSON Schema**

```json
{
  "colors": {
    "type": "array",
    "minItems": 1,
    "maxItems": 1,
    "items": [
      {
          "type": "string"
      }
    ],
    "additionalItems": false
  }
}
```

**MSON**

```mson
+ colors: *red*, *green* (array[string], fixed)
```
implies an array, containing exactly two `string` items with arbitrary values, where "red" and "green" are the sample values.

**JSON Schema**

```json
{
  "colors": {
    "type": "array",
    "minItems": 2,
    "maxItems": 2,
    "items": [
      {
        "type": "string"
      },
      {
        "type": "string"
      }
    ],
    "additionalItems": false
  }
}
```

### 2. MSON value member with nested member types

**MSON**

```mson
+ components (array, fixed)
    + (object)
    + (string)
```
implies a fixed-list `array` structure that MUST only contain the two items of an arbitrary `object` and `string` in that order.

**JSON Schema**

```json
{
  "components": {
    "type": "array",
    "minItems": 2,
    "maxItems": 2,
    "items": [
      {
        "type": "object"
      },
      {
        "type": "string"
      }
    ],
    "additionalItems": false
  }
}
```

**MSON**

```mson
+ colors (array, fixed)
    + red (string, sample)
    + green (string, sample)
```
implies a fixed-list `array` structure that MUST only contain the two items of the type  `string`, and the values provided are samples.

The above MSON is equivalent to
```mson
+ colors (array, fixed)
    + *red* (string)
    + *green* (string)
```

**JSON Schema**

```json
{
  "components": {
    "type": "array",
    "minItems": 2,
    "maxItems": 2,
    "items": [
      {
        "type": "string"
      },
      {
        "type": "string"
      }
    ],
    "additionalItems": false
  }
}
```

**MSON**

```mson
+ components (array, fixed)
    + (Point)
    + (Point)
```
implies an array, which MUST contain exactly 2 items, and each item should represent an element of user-defined _Named Type_ `Point`.

**JSON Schema**

```json
{
  "components": {
    "type": "array",
    "minItems": 2,
    "maxItems": 2,
    "items": [
      {
        "type": "object"
      },
      {
        "type": "object"
      }
    ],
    "additionalItems": false
  }
}
```

**MSON**

```mson
+ components (array[number], fixed, min-length="2", max-length="2")
    + (string)
    + (string)
```
implies a fixed-list `array` structure that MUST only contain exactly two items  of type `string`. Array's _Type Specification_ `number` is omitted here, because elements of _Nested Member Types_ override it with type `string`.

**JSON Schema**

```json
{
  "components": {
    "type": "array",
    "minItems": 2,
    "maxItems": 2,
    "items": [
      {
        "type": "string"
      },
      {
        "type": "string"
      }
    ],
    "additionalItems": false
  }
}
```

## Ranged-size array description variants

**MSON**

```mson
+ colors (array[string], min-length="3", max-length="5")
```
implies an array, which MUST contain at least 3 elements, but no more than 5 elements.

**JSON Schema**

```json
{
  "colors": {
    "type": "array",
    "minItems": 3,
    "maxItems": 5,
    "items": { "type": "string" }
  }
}
```

**MSON**

```mson
+ colors (array[string], max-length="5")
```
implies an array, which MUST NOT be empty and MUST include no more than 5 elements.

**JSON Schema**

```json
{
  "colors": {
    "type": "array",
    "maxItems": 5,
    "items": { "type": "string" }
  }
}
```

**MSON**

```mson
+ colors (array[string], min-length="3")
```
implies an array, which MUST contains at least 3 elements, but MAY contain more.

**JSON Schema**

```json
{
  "colors": {
    "type": "array",
    "minItems": 3,
    "items": { "type": "string" }
  }
}
```
