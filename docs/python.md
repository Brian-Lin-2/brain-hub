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

## Modular Programming

This is a programming paradigm in python that embraces composition. Another popular programming paradigm is Object Oriented Programming (OOP).

When you say `import <module>`, you aren't just importing a package/module but rather the entire `<name>.py` file. By default every python file is a module. This means any functions within that file will become usable inside the file that's importing it.

It defaults to searching the default directory -> PYTHONPATH -> standard library (builtin python libraries) -> third party libraries (pip)

**Common Imports:**

```python
import math
math.sqrt(16)

from math import sqrt
sqrt(16)

import math as m
m.sqrt(16)

# Imports everything. Can be dangerous as it can override your variables
from math import *
sqrt(16)

# This is sub-package import. Only works if your directory is a package
import utils.math as math
math.sqrt(16)

from utils.math import sqrt
sqrt(16)
```

### Custom Packages

In python any folder with `__init__.py` is considered a package

```txt
my_project/
├── main.py          <-- Your entry point
└── my_package/      <-- The directory
    ├── __init__.py  <-- The "Passport"
    ├── module_a.py
    └── module_b.py
```

This tells python to treat any file inside the folder as a module.

```python
import utils.math as math
math.sqrt(16)
```

This is an absolute import it tells python to import a file starting from the root directory. Most of the time you should only use absolute imports

```python
from .math import sqrt
sqrt(16)
```

This is a relative import in relation to where the file is from.

> You can upload a package using Python tools. Packages are uploaded to a package manager. Normally stored in a dist folder which acts as a final packaged version of your code.

### Module / Library / Package

A module is a single python file `math.py`

A package is a folder that hosts many modules (files) `urllib`

A library is a collection of packages/modules (folders/files) `requests`

They are used pretty interchangeably however. Just know that these represent modular programming.

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

> Important: uv treats certificates differently compared to pip. While pip needs to point to your system certificates, uv can simply use a --system-certs flag to point directly to your system certificates. This is ideal for corporate firewalls that are installed on your machine. Note that this doesn't work for Dockerfiles and you would need to inject certs in.

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

## Environment-Based Config

Most of the time, your code wil be deployed to all types of environments (ie. `dv`, `pd`). Depending on the type of environment, the URL/secrets/parameters will change. To fix this dependency, instead of hard-coding your values you can instead inject them into your system through a config.yaml

```txt
project/
├── config/
│   ├── config.yaml       # Common/Base settings (Default)
│   ├── config-dv.yaml    # Development overrides
│   └── config-pd.yaml    # Production overrides
├── main.py
└── config_loader.py
```

This is what a common folder structure would look like. `config.yaml` is only necessary if there's shared environment variables, but if there's no shared ones then you can simply omit it.

```python
import os, yaml

def initialize():
    env = os.getenv("APP_ENV", "dv")
    config = {}

    # Load and merge: Base then Environment-specific
    for suffix in ["", f"-{env}"]:
        path = f"config/config{suffix}.yaml"
        if os.path.exists(path):
            with open(path) as f:
                config.update(yaml.safe_load(f) or {})

    # Inject into OS environment
    for key, value in config.items():
        os.environ[key.upper()] = str(value)

    print(f"🚀 Injected {env} config into OS")

initialize()
```

A nice practice is having a config_loader.py that is responsible for injecting your variables into the os.environ which your actual script can then pull from to use.
