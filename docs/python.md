# Python

## Zen of Python

These are 19 principles of Python code that serve as a guide for making architectural decisions that prioritize long-term maintainability

### Beautiful is better than ugly

Python code should follow **PEP8** styling guide. Clean indentation, consistent naming, and logical grouping.

### Explicit is better than implicit

Always be clear with what your code is doing

```python
from module import * # Implicit (unclear)
from module import function # Explicit (clear)
from module; module.function() # Explicit (clear)
```

### Simple is better than complex, but complex is better than complicated

Try to solve problems in the easiest way possible. For problems that do require complexity, try to organize it into a clear structure that has many moving parts. Complex code is better than complicated messy code.

### Flat is better than nested

Don't get into indent hell. Use guard clauses to escape out of conditionals early.

Don't have nested modules. For nested modules use an `__init__.py` file to give off the illusion of the module being flat. It's better to have a bunch of functions in one util file and split out from there. Group by feature not by type

### Sparse is better than dense

It's better to expand your code out, so it's easier to read than cramming all the logic into one line.

### Readability counts

Make sure your functions/variables are easy to read and follow. This also applies to your code. It should be clean and elegant

### Special cases aren't enough to break the rules although practicality beats purity

NEVER break your project's design pattern. Consistency is key. If you do need to break the design, that implies your logic is flawed. Sometimes due to external limitations, you are forced to break your design rules. If so, you need to document why.

### Errors should never pass silently unless explicitly silenced

Don't use "bare" except blocks `except: pass`. Never try to hide bugs silently and instead you should log the error or raise it. The general rule is you log on non-crucial errors and raise whenever the program is in an invalid state (ie. missing a key on startup). If you do expect an error, pass it on purpose and document it.

### In the face of ambiguity, refuse the temptation to guess

If your code receives data it doesn't understand, never try to guess what the user means. Just raise an error. We should always have strict type checks on inputs.

### Now is better than never, although never is often better than right now

When starting to code, don't worry too much about design or architecture. First try getting a working dirty version out, and from there focus on cleaning up the code. However, even if its dirty/messy make sure its NOT buggy. If your code is buggy don't push it to production. It's better to lack a feature than to have a broken one.

### If the implementation is hard to explain, it's a bad idea. If the implementation is easy to explain, it may be a good idea

Simplicity is a strong signal of high-quality engineering. This means if possible, abstraction is your friend.

## Function vs Classes

Most of the times you want to use functions over classes in Python as it is easier to manage and use. The only time you would need to use a class is when you have state that you need to persist or you need to bundle a group of data/behavior.

## Composition

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

## Pydantic

This is the TypeScript of Python. While Python has built in type hints, Pydantic actually enforces it.

## Exception Handling

This is crucial to ensure production grade code doesn't break. Unhandled exceptions in your code can completely kill your process. Anytime there's code that could potentially crash your system, you must wrap it in a try catch. Make sure to use the `logging` module to record the stack trace.

### When it's fine to crash the program

Generally, it's fine to crash the program on initialization. If the program didn't start correctly, it's fine to just crash it instead of silently allowing it to continue. You can still have the code in a try-catch, but raise the Exception to ensure the program crashes.

## Testing / Debugging

### Scratchpad Pattern

Sometimes a code repo is too huge to easily test a single function you wrote. What you can do is create a `scratchpad` directory and add it to your `.gitignore`. From here you can create test files and use `sys.path` to let the script see the rest of your repo.

```python
import sys
from pathlib import Path
# Adds the root directory of your repo to the path
sys.path.append(str(Path(__file__).parent.parent))

from my_app.core.logic import test_function
print(test_function())
```

### ipdb

This is perfect for an interactive debugger. Let's you freeze time and see code at a certain state.

```python
import ipdb
ipdb.set_tract()
```

> You can use the built-in `breakpoint()` function in python and set `export PYTHONBREAKPOINT=ipdb.set_trace`. This allows you to use pythons built-in debugging functionality but with ipdb's advanced debugging features.

### Pytest

This is your classic library for automated tests. Can be used for unit tests, integration tests, and end-to-end tests.
