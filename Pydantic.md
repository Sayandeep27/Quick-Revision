# 📘 Pydantic Quick Guide (From Basics to Advanced)

---

## 📑 Table of Contents

* [Introduction](#-introduction)
* [BaseModel](#-basemodel)
* [Field](#-field)
* [Validator](#-validator)
* [Types of Validators](#-types-of-validators)
* [Pre vs Post Validators](#-pre-vs-post-validators)
* [Automatic Type Conversion](#-automatic-type-conversion)
* [Disable Type Conversion](#-disable-type-conversion)
* [Serialization & Deserialization](#-serialization--deserialization)
* [Examples](#-examples)

---

## 📌 Introduction

**Pydantic** is a Python library used for:

* Data validation
* Data parsing
* Type enforcement using Python type hints

---

## 🧱 BaseModel

* Core class in Pydantic
* Used to define data structure
* Automatically validates input

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
```

---

## 🎯 Field

* Adds validation rules and metadata
* Used for constraints

```python
from pydantic import Field

class User(BaseModel):
    username: str = Field(..., min_length=3, max_length=20)
    age: int = Field(..., ge=18, le=60)
```

### Explanation:

| Parameter    | Meaning               |
| ------------ | --------------------- |
| `...`        | Required field        |
| `min_length` | Minimum characters    |
| `max_length` | Maximum characters    |
| `ge`         | Greater than or equal |
| `le`         | Less than or equal    |

---

## 🛠 Validator

* Used for custom validation logic

```python
from pydantic import validator

class User(BaseModel):
    name: str

    @validator('name')
    def validate_name(cls, v):
        return v.upper()
```

---

## 🔄 Types of Validators

### 1. Field Validator

* Validates single field

```python
@validator('age')
def check_age(cls, v):
    if v < 18:
        raise ValueError("Too young")
    return v
```

### 2. Root Validator

* Validates multiple fields together

```python
from pydantic import root_validator

@root_validator
def check_password(cls, values):
    if values['password'] != values['confirm_password']:
        raise ValueError("Mismatch")
    return values
```

---

## ⚡ Pre vs Post Validators

### Pre Validator

* Runs before type conversion

```python
@validator('age', pre=True)
def clean_input(cls, v):
    return int(v)
```

### Post Validator

* Runs after type conversion (default)

```python
@validator('age')
def validate_age(cls, v):
    if v < 18:
        raise ValueError("Too young")
    return v
```

### Flow:

```
Input → Pre Validator → Type Conversion → Post Validator → Final Output
```

---

## 🔁 Automatic Type Conversion

Pydantic automatically converts compatible types.

```python
class User(BaseModel):
    age: int

user = User(age="25")  # Converted to int
```

---

## 🚫 Disable Type Conversion

### Using Strict Types

```python
from pydantic import StrictInt

class User(BaseModel):
    age: StrictInt
```

### Using Config (Pydantic v2)

```python
from pydantic import ConfigDict

class User(BaseModel):
    model_config = ConfigDict(strict=True)
    age: int
```

---

## 🔄 Serialization & Deserialization

### Serialization (Object → JSON)

```python
user.dict()
user.json()
```

### Deserialization (JSON → Object)

```python
data = {"name": "John", "age": 25}
user = User(**data)
```

---

## 🧪 Examples

### Complete Example

```python
from pydantic import BaseModel, Field, validator

class User(BaseModel):
    username: str = Field(..., min_length=3)
    age: int

    @validator('age')
    def check_age(cls, v):
        if v < 18:
            raise ValueError("Too young")
        return v

user = User(username="john", age=25)
print(user)
```

---

## 🧠 Summary

| Concept         | Meaning                |
| --------------- | ---------------------- |
| BaseModel       | Structure + validation |
| Field           | Constraints            |
| Validator       | Custom logic           |
| Field Validator | Single field check     |
| Root Validator  | Multi-field check      |
| Pre Validator   | Before conversion      |
| Post Validator  | After conversion       |
| Serialization   | Object → JSON          |
| Deserialization | JSON → Object          |

---

## ⭐ Notes

* Pydantic v2 uses:

  * `@field_validator`
  * `@model_validator`
* Strict mode disables automatic conversion


---
