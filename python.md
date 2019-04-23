# Python Coding style
Guideline of PEP8 and some additions. 
(Only things found in our codebase. To be optimized continously.)

***
## Import

### 3 blocks
Imports should be grouped in the following order:

1. Standard library imports.
2. Related third party imports.
3. Local application/library specific imports.

You should put a blank line between each group of imports.

```python
# NOT OK 
import zlib
from airflow import DAG

from hdl.airflow import global_default_args

from datetime import datetime
```

```python
# OK 
from datetime import datetime 

from airflow import DAG
import zlib

from hdl.airflow import global_default_args
```

### Alphabet order
```python
# NOT OK 
```

```python
# OK 
```

### Break long lines
```python
# NOT OK 
from hdl.variables import HDL_DAG_PREFIX, HDL_SCHEMA_PREFIX, HDL_WAREHOUSE_ROOT, DATA_FACTORY_V2, \
    DATA_FACTORY_V1, HDL_TASK_PREFIX, HDL_INGRESS_ROOT, HDL_ENV_NAME, LOCAL_TZ
```

```python
# OK 
from hdl.variables import (
    DATA_FACTORY_V1,
    DATA_FACTORY_V2,
    HDL_DAG_PREFIX,
    HDL_INGRESS_ROOT,
    HDL_SCHEMA_PREFIX,
    HDL_TASK_PREFIX,
    HDL_WAREHOUSE_ROOT,
    LOCAL_TZ,
)
```
***
## Name variable / classes / functions / Pipelines 

### Snake case ? camel case ?

   * Pascal case for the class name ( `MyClass`  )
   * Snake case for everything else ( `my_variable`, `my_function`)
   * ADF pipelines' and Airflow tasks' name (`Snake case`?)

### Constants
`Constants are usually defined on a module level and written in all capital letters with underscores separating words. Examples include MAX_OVERFLOW and TOTAL.`

### Explicit over implicit
i.e 

Why?

In 2016 most of text editors (vim/atom/eclipse/notepad++)
have good autocompletion, so there's no reason to force
a new developer to pause 5 secondes to reverse-engineer
a compressed variable name


```python
# NOT OK inc => increment / include ?
def inc_ctr():
    pass
```

```python
# OK
def include_category():
    pass
```

***
## How to break long lines

We don't force an arbitrary number of characters, however lines too long:
  * make it difficult to have several files next to each other
  * make it difficult when doing code review, to see which part of the line has changed
  * make it difficult to count how many arguments there are etc.

#### For array:

```python
# OK
my_array = [
    element1,
    element2,
    # ...
    element4, # /!\ note here the final `,` , so that adding a new element create a nice diff
] # alone and aligned with the opening line
```

```python
# NOT OK
my_array = [element1,
    element2,
    element3,
]
```

```python
# NOT OK
my_array = [
    element1,
    element2,
    # ...
    element4]
```

### List comprehension

```python
# NOT OK
robots_head_ids = [ robots['headid'] for robots in robots_list if robots['type'] == 'nao' ]
```

```python
# OK
robots_head_ids = [
    robots['headid']
    for robots in robots_list
    if robots['type] == 'nao'
]
```
 

#### For functions / methods definitions:
(same logic except that it's `(` and not `[`) 

Please always do multiline when it's `>=` 3 parameters
for 2 it's up to your common-sense, keep in mind readability > rules

```python
# OK
def my_methods(
    param1,
    param2,
    param3, # /!\ don't forget the final `,`
):
```

```python
# NOT OK
def my_methods(param1,
    param2,
    param3,
):
```

```python
# NOT OK
def my_methods(
    param1,
    param2,
    param3):
```

```python
# NOT OK
def my_methods(param1,
               param2,
              ):
```

***
## Comments
### Sentence
### One line comment
### First line
### None return / parameters

***
## Others

### Mix of single and double quota

### Magic number

