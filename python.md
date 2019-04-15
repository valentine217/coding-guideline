# Python Coding style

We follow pep8 + some addition where pep8 still offer some freedom
of choices

(NOT COMPLETE YET)

## Indentation

Indentation is done using 4 spaces

## How to Name variable / classes / functions


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

# OK
def include_category():
    pass
```

### Snake case ? camel case ?

   * Pascal case for the class name ( `MyClass`  )
   * Snake case for everything else ( `my_variable`, `my_function`)


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

# NOT OK
my_array = [element1,
    element2,
    element3,
]

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

# NOT OK
def my_methods(param1,
    param2,
    param3,
):

# NOT OK
def my_methods(
    param1,
    param2,
    param3):

# NOT OK

def my_methods(param1,
               param2,
              ):
```
