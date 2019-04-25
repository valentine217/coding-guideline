# Python

Guideline of [PEP8](https://www.python.org/dev/peps/pep-0008/), [PEP257](https://www.python.org/dev/peps/pep-0257/), [PEP20](https://www.python.org/dev/peps/pep-0020/), and some additions. To be optimized continously.


## Why

```
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
...
```

## Imports

### Grouping imports

[PEP8](https://www.python.org/dev/peps/pep-0008/#imports)
> Imports should be grouped in the following order:
> 
> 1. Standard library imports.
> 2. Related third party imports (```pip install```).
> 3. Local application/library specific imports.
> 
> You should put a blank line between each group of imports.

```python
# Not recommended
import pendulum
from airflow import DAG
from hdl.airflow import global_default_args
from datetime import datetime
```

```python
# Recommended 
from datetime import datetime 

from airflow import DAG
import pendulum

from hdl.airflow import global_default_args
```

### Order alphabetically

```python
# Not recommended 
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
# Recommended 
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
# Not recommended
from hdl.variables import HDL_DAG_PREFIX, HDL_SCHEMA_PREFIX, HDL_WAREHOUSE_ROOT, DATA_FACTORY_V2, \
    DATA_FACTORY_V1, HDL_TASK_PREFIX, HDL_INGRESS_ROOT, HDL_ENV_NAME, LOCAL_TZ
```

```python
# Recommended 
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

## Naming conventions

   * `PascalCase` for classes (`MyClass`)
   * `snake_case` for variables and functions (`my_variable`, `my_function`)

### Constants 

[PEP8](https://www.python.org/dev/peps/pep-0008/#constants)
> Constants are usually defined on a module level and written in all capital letters with underscores separating words.

```python
# Not recommended
tuple_fields = "hdl_target_schema_name ..."
```

```python
# Recommended
TUPLE_FIELDS = "hdl_target_schema_name ..."
```

### Avoid using abbreviations 

Since 2016, most of text editors (vim/atom/eclipse/notepad++) have good autocompletion.

```python
# Not recommended inc => increment / include ?
def inc_ctr():
    pass
```

```python
# Recommended
def include_category():
    pass
```

```python
# Not recommended creds_info => credit / credential ?
self.creds_info = azureapi.get_credential(cred_name=self.cluster_name, cred_type="hdi")
```

```python
# Recommended
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

[PEP8](https://www.python.org/dev/peps/pep-0008/#when-to-use-trailing-commas)
> `Trailing Commas` are often helpful when a list of values, arguments or imported items is expected to be extended over time. The pattern is to put each value (etc.) on a line by itself, always adding a trailing comma, and add the close parenthesis/bracket/brace on the next line. 

Therefore:

  * always do multiline when it's `>=` 3 parameters/elements;
  * for 2, it's up to your common-sense;
  * keep in mind readability > rules.

### Strings 

```python
# Not recommended
PartitionedTable = collections.namedtuple("PartitionedTable",
                                          ("hdl_target_schema_name hdl_target_table_name adf_activity_name adf_name "
                                           "adf_pipeline_name do_not_deduplicate primary_key_columns_list "
                                           "primary_key_sort_columns_list on_insert_sort_by min_adf_completion_count"))
```

```python
# Recommended
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

### Arrays

```python
# Not recommended
my_array = [element1,
    element2,
    element3,
]
```

```python
# Not recommended
my_array = [
    element1,
    element2,
    # ...
    element4]
```

```python
# Recommended
my_array = [
    element1,
    element2,
    # ...
    element4, # note here the final `,` , so that adding a new element create a nice diff
] # alone and aligned with the opening line
```

### Parameters 

```python
# Not recommended
def my_methods(param1,
    param2,
    param3,
):
```

```python
# Not recommended
def my_methods(
    param1,
    param2,
    param3):
```

```python
# Not recommended
def my_methods(param1,
               param2,
              ):
```

```python
# Recommended
def my_methods(
    param1,
    param2,
    param3, # don't forget the final `,`
):
```

### List comprehensions

[PEP8](https://www.python.org/dev/peps/pep-0008/#other-recommendations)
> While sometimes it's okay to put an if/for/while with a small body on the same line, never do this for multi-clause statements. Also avoid folding such long lines!

```python
# Not recommended
articles_ids = [ articles["id"] for articles in articles_list if articles["type"] == "A" ]
```

```python
# Recommended
articles_ids = [
    articles["headid"]
    for articles in articles_list
    if articles["type"] == "A"
]
```

## Comments

### Write complete sentences

[PEP8](https://www.python.org/dev/peps/pep-0008/#comments)
> Comments should be complete sentences. The first word should be capitalized.

```python
# Not recommended
# defining skip function
def skip_fn(*args, **kwargs):
    ...
```

```python
# Recommended
# Define a skip function.
def skip_fn(*args, **kwargs):
    ...
```

### One-line Docstrings

[PEP257](https://www.python.org/dev/peps/pep-0257/#one-line-docstrings)
> One-liners are for really obvious cases. They should really fit on one line.

```python
# Not recommended
def skip_fn(*args, **kwargs):
    """
    Define a placeholder function for the skip operator.
    """
    return True
```

```python
# Recommended
def skip_fn(*args, **kwargs):
    """Define a placeholder function for the skip operator."""
    return True
```

### Multi-line Docstrings

[PEP8](https://www.python.org/dev/peps/pep-0008/#comments)
> Block comments generally consist of one or more paragraphs built out of complete sentences, with each sentence ending in a period.

[PEP257](https://www.python.org/dev/peps/pep-0257/#multi-line-docstrings)
> Multi-line docstrings consist of:
> * a summary line just like a one-line docstring, 
> * followed by a blank line, 
> * followed by a more elaborate description. 
> The summary line may be used by automatic indexing tools; it is important that it fits on one line and is separated from the rest of the docstring by a blank line.

```python
# Not recommended 
def compute_params(context):
    """
    Passing date and environment parameters to ADF Pipeline
    :param context: ...
    :return: pDayPath, pMonthPath, pYearPath, pDatePath
```

```python
# Recommended
def compute_params(context):
    """Pass the date and environment parameters to ADF Pipeline.
    
    :param context: ...
    :return: pDayPath, pMonthPath, pYearPath, pDatePath
```

### None return or parameters

```python
# Not recommended
def skip_fn(*args, **kwargs):
    """Placeholder function for the skip operator.
    :param args:
    :param kwargs:
    """
    return True
```

```python
# Recommended
def skip_fn(*args, **kwargs):
    """Placeholder function for the skip operator."""
    return True
```

## Miscellaneous

### Mix of single and double quote

[PEP8](https://www.python.org/dev/peps/pep-0008/#string-quotes)
> In Python, single-quoted strings and double-quoted strings are the same. This PEP does not make a recommendation for this. Pick a rule and stick to it to avoid backslashes. It improves readability.

```python
# Not recommended
query = {"query": 'metrics_resourcemanager_clustermetrics_CL'
                  '| where ClusterType_s == "spark" and TimeGenerated > ago(5m) and ClusterName_s '
                  'contains ' "\"" + CLUSTER_NAME + "\""
                  '| sort by AggregatedValue desc| where AggregatedValue > 0'}
```

```python
# Recommended
query = """metrics_resourcemanager_clustermetrics_CL
| where ClusterType_s == "spark" and TimeGenerated > ago(5m) and ClusterName_s contains "{CLUSTER_NAME}"
| sort by AggregatedValue desc| where AggregatedValue > 0
""".format(CLUSTER_NAME=CLUSTER_NAME)
```

### [Magic number](https://en.wikipedia.org/wiki/Magic_number_(programming))

Magic numbers makes the code difficult:

  * to read and understand;
  * to alter the value of the number, as it is not duplicated. 

```python
# Not recommended
LOAD = HDLPartitionedDecentralizedDeltaInsertOperator(
    num_partitions_per_batch=100,
    max_partitions_in_total=600,
    megabytes_per_reducer_on_finalize=128,
)
```

```python
# Recommended
LOAD = HDLPartitionedDecentralizedDeltaInsertOperator(
    num_partitions_per_batch=PARTITIONED_TABLE.num_partitions_per_batch,
    max_partitions_in_total=PARTITIONED_TABLE.max_partitions_in_total,
    megabytes_per_reducer_on_finalize=PARTITIONED_TABLE.megabytes_per_reducer_on_finalize,
)
```

### End with a blank line

```python
# Not recommended
from airflow import DAG

with DAG(...) as dag:
    loo = ...
    insert = ...
    loo >> insert 

    dag.doc_md = __doc__ # The end of the file is here.
```

```python
# Recommended
from airflow import DAG

with DAG(...) as dag:
    loo = ...
    insert = ...
    loo >> insert 

    dag.doc_md = __doc__
# Leave a new blank line with no content here as the end of the file. 
```
