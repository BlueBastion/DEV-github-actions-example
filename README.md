# Github Actions Intro

## Guide  
Intro steps are made in branches to develop a new idea, one at a time.

- 00: [Starting Branch](https://github.com/BlueBastion/DEV-github-actions-example/tree/00-start)
- 01: [Context Variables](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts)
- 02: [Running a Script](https://github.com/BlueBastion/DEV-github-actions-example/tree/02-running-a-script)

# Github Contexts
[This workflow]() demonstrates how to use context objects in GitHub Actions. 
Contexts provide information about the workflow run, such as the event that triggered the workflow, the job being executed, and the runner environment. 
We are going to dump these contexts to the log, so that we can inspect their contents and understand how to use them in our workflows.

## Workflow Overview
- **Name**: `Context testing`
- **Trigger**: This workflow runs on every `push` event.
- **Jobs**: 
  - `dump_contexts_to_log`: This job runs on the latest Ubuntu runner and dumps various context objects to the log.

---

## Key Concepts
### Context Objects
Contexts are objects that contain information about the workflow run, such as:
- **`github`**: Information about the GitHub event that triggered the workflow (e.g., `push`, `pull_request`).
- **`job`**: Information about the current job being executed.
- **`steps`**: Information about the steps in the current job.
- **`runner`**: Information about the runner environment (e.g., OS, temp directory).
- **`strategy`**: Information about the matrix strategy (if used).
- **`matrix`**: Information about the matrix configuration (if used).

### `toJson` Function
The `toJson` function is used to convert context objects into a JSON string, making it easier to inspect their contents.
GitHub functions are documented [here](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/evaluate-expressions-in-workflows-and-actions#functions)

---

## Workflow Breakdown

### Workflow Definition
```yaml
name: Context testing
on: push
```

- **`name`**: The name of the workflow, which will be displayed in the GitHub Actions UI.
- **`on: push`**: The workflow is triggered on every `push` event.

---

### Job Definition
```yaml
jobs:
  dump_contexts_to_log:
    runs-on: ubuntu-latest
    steps:
```

- **`jobs`**: Defines the jobs that will be executed as part of the workflow.
- **`dump_contexts_to_log`**: The name of the job. You can name jobs anything, as long as it’s a valid YAML key.
- **`runs-on: ubuntu-latest`**: Specifies that the job will run on the latest Ubuntu runner.

---

### Steps
Each step in the job dumps a specific context object to the log using the `echo` command.

We are going to outline only the first, as they are all the same
#### 1. Dump GitHub Context
```yaml
      - name: Dump GitHub context
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: echo "$GITHUB_CONTEXT"
```

- **`name`**: A descriptive name for the step.
- **`env`**: Defines an environment variable (`GITHUB_CONTEXT`) that stores the JSON representation of the `github` context.
- **`run`**: Executes a shell command to print the contents of the `GITHUB_CONTEXT` variable.

***NOTE:*** When using BASH variables `$GITHUB_CONTEXT` the  only reason why this is defined is because the step 
has defined the variable in `env: GITHUB_CONTEXT: ...`  GITHUB_CONTEXT is not predefined in the bash environment started
by `run:`

---

## Why Use Contexts?
Contexts are useful for:
- **Debugging**: Inspecting the contents of context objects can help you debug workflows.
- **Dynamic Behavior**: You can use context information to dynamically control workflow behavior (e.g., running certain steps only on specific branches).
- **Logging**: Logging context information can provide insights into the workflow execution environment.

---

## Next Steps
- Experiment with other context objects, such as `env` or `secrets`.
- Use context information to conditionally execute steps or jobs.
- Explore the [GitHub Actions documentation](https://docs.github.com/en/actions/learn-github-actions/contexts) for more details on available contexts.

## Next Section

[Back](https://github.com/BlueBastion/DEV-github-actions-example/tree/00-start) |
[Running a Script](https://github.com/BlueBastion/DEV-github-actions-example/tree/02-running-a-script)
