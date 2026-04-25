# JSON

## JSON Schema

Used to describe exactly what a JSON document should look like.

```python
import json
from jsonschema import validate, ValidationError

# 1. This would normally be loaded from a file or your Azure Schema Registry
my_schema = {
    "type": "object",
    "properties": {
        "model_id": {"type": "string"},
        "accuracy": {"type": "number", "minimum": 0, "maximum": 1}
    },
    "required": ["model_id"]
}

# 2. The data you want to check
incoming_data = {"model_id": "resnet-50", "accuracy": 0.98}

# 3. Perform the check
try:
    validate(instance=incoming_data, schema=my_schema)
    print("Validation Successful!")
except ValidationError as e:
    print(f"Validation Failed: {e.message}")
```
