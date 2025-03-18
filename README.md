# Github Actions Intro

## Guide  
Intro steps are made in branches to develop a new idea, one at a time.

- 00: [Starting Branch](https://github.com/BlueBastion/DEV-github-actions-example/tree/00-start)
- 01: [Context Variables](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts)
- 02: [Running a Script](https://github.com/BlueBastion/DEV-github-actions-example/tree/02-running-a-script)

# Start  
## Review  
### The Definition
Definition files must be located in `.github/workflows/` and be `yaml` files.
There is one [workflow file](.github/workflows/test-action.yml) in this branch.

### Workflow Name
A top level name key will determine how the github gui will display the name of your action and/or a badge if you create one.
```yaml
name: My-Big-Ol-Workflow
```

### Triggers
A number of Triggers will determine when your action will run.  The documentation can be reviewed [here](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows).  
For the purpose of this introduction we will be using the following trigger:
```yaml
on:
  push:
```



- One of the most important things to understand about Github actions is that anything you do in them starts from zero, always.
You start every action with a clean slate and must build your environment before you perform any task.

- In the 01 branch we will review what context variables are provided by default by the github runner.

- 



