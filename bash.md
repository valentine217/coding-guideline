#Bash script coding style

## Indentation

Indentation is 4 spaces

## Common best practices

Add at the beginning, or as early as possible add: 

```bash
set -eu
```

  * `-e` permits to quit as soon as one command fails
  * `-u` fails as soon as a variable non defined is used (e.g to avoid doing `rm -rf $TEMPRARY_DIR/`
  (note the typo) that will `rm -rf /` )
