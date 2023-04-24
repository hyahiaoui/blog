---
title: "Migrating from Makefile to Taskfile"
date: 2023-04-20T19:00:00+02:00
draft: true
---

I discovered the joy of using [Task](https://taskfile.dev/) (also know as `Taskfile`), not too long ago. I'm enjoying this new tool, more and more. And I started using it, and pushing for its use, in a lot of the repositories I contribute to.

## Makefile as a task runner

Until now, I always used `make` as a task runner, in almost all my repositories. The `Makefile` always contained some "magic", to make using the tool more enjoyable and easier.

For example, I always introduced the ability to have a self-documenting Makefile by creating a `help` target that lists the available targets in the `Makefile` and print the associated one-line documentation. 

The minimal `Makefile` looked like

```makefile
.DEFAULT_GOAL := help

.PHONY: help
help:
    @grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-35s\033[0m %s\n", $$1, $$2}'

task1:  ## Documentation of the task 
    @echo "Do stuff"
```

This allowed running a simple `make` command to discover the targets available, and their roles:

```bash
$ make  # or make help
task1                               Documentation of the task 
```

## Introducing Taskfile

