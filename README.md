# GitHub Actions Intro

## Guide  
Intro steps are made in branches to develop a new idea, one at a time.

- 00: [Starting Branch](https://github.com/BlueBastion/DEV-github-actions-example/tree/00-start)
- 01: [Context Variables](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts)
- 02: [Running a Script](https://github.com/BlueBastion/DEV-github-actions-example/tree/02-running-a-script)

# Start  
**One of the most important things to understand about GitHub actions is that anything you do in them starts from zero, always.**
**You start every action with a clean slate and must build your environment before you perform any task.**

## Goals
- In this branch we will review the basics of creating an action.
- In the 01 branch we will review what context variables are provided by default by the github runner.
- In the 02 branch, we will run a bash script located in the repository,
and explore the concepts of `GITHUB_OUTPUT`, and the `GITHUB_STEP_SUMMARY`

## Concepts
Key Concepts
- Workflow: A configurable automated process defined in a YAML file.
- Event: A specific activity that triggers a workflow (e.g., push, pull_request).
- Job: A set of steps that execute on the same runner (e.g., build, test, deploy).
- Step: An individual task within a job (e.g., run a command, use an action).
- Action: A reusable unit of code that performs a specific task (e.g., checkout code, install dependencies).
- Runner: A machine (virtual or physical) that executes jobs.

## Explanation  
### The Definition
Definition files must be located in `.github/workflows/` and be `yaml` files.
There is one [workflow file](.github/workflows/test-action.yml) in this branch.  
You may view actions runs for this workflow [here](https://github.com/BlueBastion/DEV-github-actions-example/actions/workflows/test-action.yml). Or with `gh run view`, which can be a very useful tool.

### Workflow Name
A top level name key will determine how the github gui will display the name of your action and/or a badge, if you create one.
```yaml
name: GitHub Actions Demo
```

### Triggers
A number of Triggers will determine when your action will run.  The documentation can be reviewed [here](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows).  
For the purpose of this introduction we will be using the following trigger:  
```yaml
on:
  push:
  workflow_dispatch:
```
This is a valid trigger definition. No child keys are required under push, but there are [some options you can use](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/triggering-a-workflow#using-activity-types-and-filters-with-multiple-events).  
We will also add and use "workflow_dispatch" so that we can trigger a job via a button in the github gui.  However, it is recommended that actions, *not* be run manually.

### Variables
We can notice that there are variables in this action and their values are retrieved by using `${{ variable-name }}`.  For example, on [line 2](https://github.com/BlueBastion/DEV-github-actions-example/blob/352112f25c40705f2b74d452f6574093411016e3/.github/workflows/test-action.yml#L2)
of our action, there is `${{ github.actor }}`.  This is a context object provided by github, which we will review in the [next section](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts).

### Jobs
One main key to take note of is the `jobs` key.  This defines what "jobs" will be run as part of this workflow.  Jobs can be named anything so long as it is a valid yaml key.
You may choose environment variables, defaults, and a runner-type in the child keys of a job. You must define a `steps` list and a `runs-on` value. The others are optional
```yaml
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
```

We now have all the parts and pieces of a valid workflow. Putting it all together we have:
```yaml
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on:
    push: 
    workflow_dispatch:
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
```
We will add a few more run commands in our action.  Be sure to check out some of the outputs in [the action runs](https://github.com/BlueBastion/DEV-github-actions-example/actions/workflows/test-action.yml).

### Next Section
[Context Variables](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts)
