# Python
Guideline of PEP8, PEP257, and some additions. 
(Only things found in our codebase. To be optimized continously.)


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
from hdl.variables import HDL_DAG_PREFIX, HDL_TASK_PREFIX, HDL_ENV, HDL_WAREHOUSE_ROOT, HDL_SCHEMA_PREFIX, \
    DATA_FACTORY_V2
from hdl.variables import HAAL_ADL_ROOT
from hdl.airflow.sensors.adf_activity_run_sensor import ADFActivityRunSensor
from hdl.py.dl_raw import partitioned as load_partitioned
from hdl.airflow.operators.hdl_hive_operator import HDLHiveOperator
from hdl.airflow.sensors.hdl_table_source_file_is_ready_sensor import HDLTableSourceFileIsReadySensor
from hdl.py.dl_raw import unpartitioned as load_unpartitioned
from hdl.airflow.operators.hdl_access_rights_operator import HDLAccessRightsOperator
from hdl.airflow.operators.hdl_load_status_operator import HDLLoadStatusOperator
from hdl.airflow.operators.hdl_compute_stats_operator import HDLComputeStatsOperator
from hdl.airflow import global_default_args
```

```python
# OK 
from hdl.airflow import global_default_args
from hdl.airflow.operators.hdl_access_rights_operator import HDLAccessRightsOperator
from hdl.airflow.operators.hdl_compute_stats_operator import HDLComputeStatsOperator
from hdl.airflow.operators.hdl_hive_operator import HDLHiveOperator
from hdl.airflow.operators.hdl_load_status_operator import HDLLoadStatusOperator
from hdl.airflow.sensors.adf_activity_run_sensor import ADFActivityRunSensor
from hdl.airflow.sensors.hdl_table_source_file_is_ready_sensor import HDLTableSourceFileIsReadySensor
from hdl.py.dl_raw import partitioned as load_partitioned
from hdl.py.dl_raw import unpartitioned as load_unpartitioned
from hdl.variables import ...
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

## Name variable / classes / functions / pipelines 

### Snake case or Camel case 

   * Pascal case for the class name ( `MyClass`  )
   * Snake case for everything else ( `my_variable`, `my_function`)
   * ADF pipelines' and Airflow tasks' name (`Snake case`?)

### Constants
Constants are usually defined on a module level and written in all capital letters with underscores separating words.

```python
# NOT OK 
tuple_fields = "hdl_target_schema_name ..."

```

```python
# OK
TUPLE_FIELDS = "hdl_target_schema_name ..."
```

### Explicit over implicit
Why? In 2016 most of text editors (vim/atom/eclipse/notepad++) have good autocompletion, so there's no reason to force
a new developer to pause 5 secondes to reverse-engineer a compressed variable name.

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

```python
# NOT OK creds_info => credit / credential ?
self.creds_info = azureapi.get_credential(cred_name=self.cluster_name, cred_type="hdi")
```

```python
# OK
self.credential_info = azureapi.get_credential(
    credential_name=self.cluster_name, 
    credential_type="hdi",
)
```

## How to break long lines
We don't force an arbitrary number of characters; however, lines too long makes it difficult:
  * to have several files next to each other
  * when doing code review, to see which part of the line has changed
  * to count how many arguments there are etc.

What we cloud do:
  * Always do multiline when it's `>=` 3 parameters/elements;
  * For 2, it's up to your common-sense;
  * Please keep in mind readability > rules.

#### For array:
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

```python
# OK
my_array = [
    element1,
    element2,
    # ...
    element4, # /!\ note here the final `,` , so that adding a new element create a nice diff
] # alone and aligned with the opening line
```

#### For functions / methods definitions: 
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

```python
# OK
def my_methods(
    param1,
    param2,
    param3, # /!\ don't forget the final `,`
):
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

## Comments
### Sentence
```python
# NOT OK 

```

```python
# OK

```
### One line comment
```python
# NOT OK 

```

```python
# OK

```
### First line
```python
# NOT OK 

```

```python
# OK

```
### None return / parameters
```python
# NOT OK 

```

```python
# OK

```


## Others

### Mix of single and double quota

### Magic number

