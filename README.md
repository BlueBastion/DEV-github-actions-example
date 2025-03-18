# Github Actions Intro

## Guide  
Intro steps are made in branches to develop a new idea, one at a time.

- 00: [Starting Branch](https://github.com/BlueBastion/DEV-github-actions-example/tree/00-start)
- 01: [Context Variables](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts)
- 02: [Running a Script](https://github.com/BlueBastion/DEV-github-actions-example/tree/02-running-a-script)

# Running a Script Workflow

This workflow demonstrates how to run a shell script in a GitHub Actions workflow and use **GitHub Outputs** and **Step Summaries** to capture and display script results. This is particularly useful for automating tasks, running custom scripts, and sharing results across steps or jobs.

---

## Workflow Overview
- **Name**: `Running a script`
- **Trigger**: This workflow runs on every `push` event.
- **Jobs**: 
  - `run-a-script`: This job runs on the latest Ubuntu runner and executes a shell script located in the repository.

---

## Key Concepts
### GitHub Outputs
GitHub Outputs allow you to pass data between steps in a job. You can define outputs using the `$GITHUB_OUTPUT` environment variable. Outputs are stored as key-value pairs and can be accessed in subsequent steps.

### Step Summaries
Step Summaries allow you to add custom Markdown content to the summary of a workflow run. This is useful for displaying results, logs, or other information directly in the GitHub Actions UI.

---

## Workflow Breakdown

### Workflow Definition
```yaml
name: Running a script
on: push
```

- **`name`**: The name of the workflow, which will be displayed in the GitHub Actions UI.
- **`on: push`**: The workflow is triggered on every `push` event.

---

### Job Definition
```yaml
jobs:
  run-a-script:
    runs-on: ubuntu-latest
    steps:
```

- **`jobs`**: Defines the jobs that will be executed as part of the workflow.
- **`run-a-script`**: The name of the job. You can name jobs anything, as long as it’s a valid YAML key.
- **`runs-on: ubuntu-latest`**: Specifies that the job will run on the latest Ubuntu runner.

---

### Steps
Each step in the job performs a specific task, such as checking out the repository, running a script, or capturing script output.

#### 1. Checkout Code
```yaml
      - uses: actions/checkout@v4
```

- **`uses: actions/checkout@v4`**: This step checks out the repository code so that the workflow can access it.

#### 2. List Directory Contents
```yaml
      - name: lets check what directory we're in
        run: ls -la 
```

- **`name`**: A descriptive name for the step.
- **`run: ls -la`**: Lists the contents of the current directory. This is useful for debugging and verifying the working directory.

#### 3. Run the Script (No Output)
```yaml
      - name: Run the script, no output.
        run: scripts/runme.sh 'an-argument'
```

- **`name`**: A descriptive name for the step.
- **`run: scripts/runme.sh 'an-argument'`**: Executes the `runme.sh` script located in the `scripts` directory, passing `'an-argument'` as an argument. The script runs, but its output is not captured.

#### 4. Run the Script with GitHub Output
```yaml
      - name: run script with Github Output.  Must be key=value pairs.
        id: script_results
        run: echo "results=$(scripts/runme.sh 'to-output')" >> $GITHUB_OUTPUT
```

- **`name`**: A descriptive name for the step.
- **`id: script_results`**: Assigns an ID to this step so that its output can be referenced later.
- **`run`**: Runs the `runme.sh` script with the argument `'to-output'` and captures its output in the `results` key. The output is stored in `$GITHUB_OUTPUT` for use in subsequent steps.

#### 5. Use Defined Outputs to Add to Step Summary
```yaml
      - name: Use defined outputs to add to step summary
        env:
          LAST_RESULT: ${{  steps.script_results.outputs.results  }}
        run: |
          echo $LAST_RESULT >> $GITHUB_STEP_SUMMARY
```

- **`name`**: A descriptive name for the step.
- **`env`**: Defines an environment variable (`LAST_RESULT`) that stores the output from the `script_results` step.
- **`run`**: Appends the value of `LAST_RESULT` to the step summary using the `$GITHUB_STEP_SUMMARY` environment variable. This allows the script output to be displayed in the GitHub Actions UI.

---

## Example Output
When this workflow runs, the log will display:
1. The contents of the current directory (from `ls -la`).
2. The output of the `runme.sh` script (both with and without capturing output).
3. The captured output (`results`) in the step summary.

For example, the step summary might look like this:
```
Script output: Hello, to-output!
```

---

## Why Use This Workflow?
This workflow is useful for:
- **Automating Script Execution**: Run custom scripts as part of your CI/CD pipeline.
- **Capturing Output**: Use GitHub Outputs to pass data between steps.
- **Displaying Results**: Use Step Summaries to provide clear, actionable feedback in the GitHub Actions UI.

---

## Next Section
