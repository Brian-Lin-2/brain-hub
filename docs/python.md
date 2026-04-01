# Python

## Design

Composition is the new design approach of building complex objects by combining smaller, simpler objects. It's main concept is a 'Has-A' relationship.

ie. A car has a battery, engine, wheels. You can swap out the batteries with new ones and the car can still work as intended

```python
class ContentManager:
    def __init__(self, formatter, publisher):
        # Composition: This class "Has-A" formatter and "Has-A" publisher
        self.formatter = formatter
        self.publisher = publisher

    def create_post(self, title):
        formatted_text = self.formatter.format(title)
        self.publisher.publish(formatted_text)
```

In this example, the ContentManager class doesn't know how to format or publish things. However, it has a formatter and publisher which performs all that logic for it.

Real World Example: SDKs (Software Development Kits) are built on the concept of composition

```python
# You are using COMPOSITION to set up the SDK
from azure.identity import DefaultAzureCredential
from azure.storage.blob import BlobServiceClient

# 1. Create the 'Credential' brick
cred = DefaultAzureCredential()

# 2. 'Compose' the Service Client by giving it the credential brick
client = BlobServiceClient(account_url="...", credential=cred) # You can swap out for another credential at any time
```

## Virtual Environment

Self-contained directory that keeps all dependencies of a specific project separate from other projects and the global system.

**Creating a virtual environment:**

1. `python -m venv .venv`
2. `source .venv/bin/activate`

## Requirements.txt

This is where Python hosts all its dependencies. You can control all the packages and versioning here. By default this pulls from PyPI (python's official third-party software). Pip is the package manager for python

**Helpful Notes:**

`pip install -r requirements.txt` - will install all packages into your virtual environment
`pip freeze > requirements.txt` - will look at your current environment and write all the packages into requirements.txt
`--index-url <package-index-url>` - allows you to use different package indexes

> Always add .venv into your .gitignore

## Modern Approach

`uv` is a modern tool designed to replace both pip and venv. Incredibly fast when downloading. It will create a uv.lock file. All previous pip commands can be used with uv prefixed and have huge performance boosts.

`pyproject.toml` is a modern version that combines `setup.py`, `setup.cfg`, `requirements.txt`, `MANIFEST.in`

**Helpful Notes:**

`uv venv` - creates a uv virtual environment
`uv pip install <package>` - faster version of pip
`uv run <file>.py` - this is a combination of `source .venv/bin/activate` and `python <file>.py`

`pyproject.toml` -

```toml
[project]
name = "brain-hub"
version = "0.1.0"
description = "A minimalist knowledge base for engineering and design"
readme = "README.md"
requires-python = ">=3.12"
license = { text = "MIT" }
authors = [
    { name = "Brian", email = "brian@example.com" }
]

# This replaces your old requirements.txt
dependencies = [
    "fastapi>=0.109.0",
    "uvicorn[standard]>=0.27.0",
    "pydantic>=2.6.0",
]

[tool.uv]
# These are only installed during development (not in production)
dev-dependencies = [
    "ruff>=0.3.0",   # Fast linter/formatter
    "pytest>=8.0.0", # Testing framework
    "mypy>=1.8.0",   # Static type checking
]

[tool.ruff]
# You can even configure your linter right here!
line-length = 88
select = ["E", "F", "I"] # Error, Pyflakes, and Isort (auto-sorting imports)

[build-system]
requires = ["hatchling"]
build-backend = "hatchling"
```
