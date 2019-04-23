# Python
Guideline of [PEP8](https://www.python.org/dev/peps/pep-0008/), [PEP257](https://www.python.org/dev/peps/pep-0257/), and some additions. To be optimized continously.


## Import

### 3 groups 
Imports should be grouped in the following order:

1. Standard library imports.
2. Related third party imports (```pip install```).
3. Local application/library specific imports.

You should put a blank line between each group of imports.

```python
# NOT OK 
import pendulum
from airflow import DAG
from hdl.airflow import global_default_args
from datetime import datetime
```

```python
# OK 
from datetime import datetime 

from airflow import DAG
import pendulum

from hdl.airflow import global_default_args
```

### Alphabet order for each group
```python
# NOT OK 
from hdl.variables import HDL_DAG_PREFIX, ...
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
from hdl.variables import HDL_DAG_PREFIX, ...
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

## Break long lines
We don't force an arbitrary number of characters; however, lines too long makes it difficult:
  * to have several files next to each other
  * when doing code review, to see which part of the line has changed
  * to count how many arguments there are etc.

What we cloud do:
  * always do multiline when it's `>=` 3 parameters/elements;
  * for 2, it's up to your common-sense;
  * keep in mind readability > rules.

### For long string 
```python
# NOT OK
PartitionedTable = collections.namedtuple("PartitionedTable",
                                          ("hdl_target_schema_name hdl_target_table_name adf_activity_name adf_name "
                                           "adf_pipeline_name do_not_deduplicate primary_key_columns_list "
                                           "primary_key_sort_columns_list on_insert_sort_by min_adf_completion_count"))
```

```python
# OK
PartitionedTable = collections.namedtuple(
    "PartitionedTable",
    " ".join([
        "adf_name",
        "adf_activity_name",
        "adf_pipeline_name",
        "do_not_deduplicate",
        "hdl_target_schema_name",
        "hdl_target_table_name",
        "min_adf_completion_count",
        "on_insert_sort_by",
        "primary_key_columns_list",
        "primary_key_sort_colums_list",
    ])
)
```

### For array
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
    element4, # note here the final `,` , so that adding a new element create a nice diff
] # alone and aligned with the opening line
```

### For functions / methods definitions 
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
    param3, # don't forget the final `,`
):
```

### For list comprehension
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
# defining skip function
def skip_fn(*args, **kwargs):
    ...
```

```python
# OK
# Define a skip function.
def skip_fn(*args, **kwargs):
    ...
```

### One line comment
```python
# NOT OK 
def skip_fn(*args, **kwargs):
    """
    Define a placeholder function for the skip operator.
    """
    return True
```

```python
# OK
def skip_fn(*args, **kwargs):
    """Define a placeholder function for the skip operator."""
    return True
```

### First line
```python
# NOT OK 
def compute_params(context):
    """
    Passing date and environment parameters to ADF Pipeline
    :param context: ...
    :return: pDayPath, pMonthPath, pYearPath, pDatePath
```

```python
# OK
def compute_params(context):
    """Pass the date and environment parameters to ADF Pipeline.
    
    :param context: ...
    :return: pDayPath, pMonthPath, pYearPath, pDatePath
```

### None return / parameters
```python
# NOT OK 
def skip_fn(*args, **kwargs):
    """Placeholder function for the skip operator.
    :param args:
    :param kwargs:
    """
    return True
```

```python
# OK
def skip_fn(*args, **kwargs):
    """Placeholder function for the skip operator."""
    return True
```

## Others
### Mix of single and double quote
In Python, single-quoted strings and double-quoted strings are the same. This PEP does not make a recommendation for this. Pick a rule and stick to it to avoid backslashes. It improves readability.

```python
# NOT OK 
query = {"query": 'metrics_resourcemanager_clustermetrics_CL'
                  '| where ClusterType_s == "spark" and TimeGenerated > ago(5m) and ClusterName_s '
                  'contains ' "\"" + CLUSTER_NAME + "\""
                  '| sort by AggregatedValue desc| where AggregatedValue > 0'}
```

```python
# OK
query = """metrics_resourcemanager_clustermetrics_CL
| where ClusterType_s == "spark" and TimeGenerated > ago(5m) and ClusterName_s contains "{CLUSTER_NAME}"
| sort by AggregatedValue desc| where AggregatedValue > 0
""".format(CLUSTER_NAME=CLUSTER_NAME)
```

### [Magic number](https://en.wikipedia.org/wiki/Magic_number_(programming))
```Magic Number``` makes the code different:
* to read and understand;
* to alter the value of the number, as it is not duplicated. 

```python
# NOT OK
LOAD = HDLPartitionedDecentralizedDeltaInsertOperator(
    num_partitions_per_batch=100,
    max_partitions_in_total=600,
    megabytes_per_reducer_on_finalize=128,
)
```

```python
# OK
PARTITIONED_TABLE = PartitionedTable(
    num_partitions_per_batch=100,
    max_partitions_in_total=600,
    megabytes_per_reducer_on_finalize=128,
)
LOAD = HDLPartitionedDecentralizedDeltaInsertOperator(
    num_partitions_per_batch=PARTITIONED_TABLE.num_partitions_per_batch,
    max_partitions_in_total=PARTITIONED_TABLE.max_partitions_in_total,
    megabytes_per_reducer_on_finalize=PARTITIONED_TABLE.megabytes_per_reducer_on_finalize,
)
```
