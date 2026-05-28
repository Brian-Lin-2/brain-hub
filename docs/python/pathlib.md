# Pathlib

Pathlib is a modern python module for handling file paths.

```python
from pathlib import Path

base_dir = Path("Documents") # This assumes the folder/file is in the current working directory
sub_file = base_dir / "projects" / "file.py" # Documents/projects/file.py

path = Path("Users/alex/reports/monthly_budget.xlsx")

print(path.name)    # "monthly_budget.xlsx" (The full filename)
print(path.stem)    # "monthly_budget"      (The name without extension)
print(path.suffix)  # ".xlsx"               (The extension)
print(path.parent)  # Users/alex/reports    (The folder it lives in)
```

**Useful Actions:**

`exists()` - Checks if a file exists
`is_file()` - Checks if the current path is a file
`is_dir()` - Checks if the current path is a folder
`mkdir()` - Creates a new directory
`read_text()` - Reads a file and its contents
`glob()` - Searches for regex in the immediate folder
`rglob()` - Searches for regex in the immediate folder and all subfolders
`rename()` - Renames a file (returns a new Path object)
`unlink()` - Deletes a file
`rmdir()` - Deletes an empty folder
