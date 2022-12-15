# Creating a Python Package

## How to Package?

### 1 - Install Hatch (Already done)

```bash
pip install hatch
```

### 2 - Edit hatch config (optional)

```bash
code $(hatch config find)
```

- under `[template.licenses]`
    - set `headers=false`
    - set `default=[]`
- under `[template.plugins.default]` set
    - `src-layout = true`

### 3 - Use hatch to create project

```bach
hatch new traffic_light_counter
```

This creates a new folder called `traffic_light_counter`, but we actually want to copy the contents out of the folder. You can do it manually, or paste the below into the terminal;

```bash
rm traffic-light-counter/README.md; mv traffic-light-counter/* .; rmdir traffic-light-counter/
```
### 4 - Open `pyproject.toml`

- Add pandas dependency​ `dependencies = ["pandas"]`

### 5 - Run Hatch Shell

```bash
hatch shell
```

### 6 - Test everythign is working

```bash
hatch run cov
```

### 7 - Create `.gitignore` file

With the following contents

```gitignore
.pytest_cache
__pycache__
dist
```

### 8 - Copy paste code

Copy copy the code below into `/src/traffic_light_counter/count_red_amber_green.py`

```python
def count_red_amber_green(column):
    column = column.str.lower().str.strip()
    return {
        "red"   : (
            column.str.endswith("red"  )
            | column.str.startswith("red"  )
        ).sum(),
        "amber" : (
            column.str.endswith("amber")
            | column.str.startswith("amber")
        ).sum(),
        "green" : (
            column.str.endswith("green")
            | column.str.startswith("green")
        ).sum(),
    }
```

### 9 - Modify `__init__.py`

Copy copy the code below into `/src/traffic_light_counter/__init__.py`

```python
from .count_red_amber_green import count_red_amber_green
```

### 10 - Copy Paste Tests

Copy the code below to a new file in the `tests` folder:

`tests/test_count_red_amber_green.py`

```python
def test_count_red_amber_green():
    import pandas as pd
    from traffic_light_counter import count_red_amber_green
    input = pd.Series([
        "Amber",
        "---red",
        "red--",
        "green",
        "green ",
    ])
    expected_result = {"red": 2, "amber": 1, "green": 2}
    actual_result = count_red_amber_green(input)
    assert actual_result==expected_result
```

### 11 - Run Tests

Get hatch to run tests, including code coverage report;

```bash
hatch run cov
```
<details>

> NOTE: having problems here
> to fix try check pyproject.toml is correct:

```
[tool.hatch.version]
path = "src/traffic_light_counter/__about__.py"
```

then run

```bash
exit
hatch env purge
hatch shell
```

</details>


### 12 - Build wheel :)

```bash
hatch build
```
### 13 - exit hatch


```bash
exit
pip install dist/traffic_light_counter-0.0.1-py3-none-any.whl
```

## Teacher's notes

<details>

Ignore  this section of the readme

Cleanup workspace

```bash
rm -r src; rm -r tests; rm pyproject.toml; rm .gitignore; rm -r -f .pytest_cache; rm -r -f dist;rm .coverage
```

</details>