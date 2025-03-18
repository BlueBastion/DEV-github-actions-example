# Github Actions Intro

## Guide  
Intro steps are made in branches to develop a new idea, one at a time.

- [TOC](https://github.com/BlueBastion/DEV-github-actions-example/tree/main)
- 01: [Previous Section](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts)
- 03: [Next Section](https://github.com/BlueBastion/DEV-github-actions-example/tree/03-prebuilt-actions)

# 02: Running a Script Workflow

[This workflow](https://github.com/BlueBastion/DEV-github-actions-example/blob/02-running-a-script/.github/workflows/run-a-script.yml) 
demonstrates how to run a shell script in a GitHub Actions workflow and use 
**GitHub Outputs** and 
**Step Summaries** to capture and display script results. 
This is particularly useful for automating tasks, running custom scripts, 
and sharing results across steps or jobs.

## Key Concepts
### GitHub Outputs
GitHub Outputs allow you to pass data between steps in a job. You can define outputs using the `$GITHUB_OUTPUT` environment variable. Outputs are stored as key-value pairs and can be accessed in subsequent steps.

### Step Summaries
Step Summaries allow you to add custom Markdown content to the summary of a workflow run. This is useful for displaying results, logs, or other information directly in the GitHub Actions UI.

## Workflow Overview
- **Name**: `Running a script`
- **Trigger**: This workflow runs on every `push` event.
- **Jobs**: 
  - `run-a-script`: This job runs on the latest Ubuntu runner and executes a shell script located in the repository.

## Step Breakdown
### Steps
Each step in the job performs a specific task, such as checking out the repository, running a script, or capturing script output.

#### 1. Checkout Code
```yaml
      - uses: actions/checkout@v4
```

- **`uses: actions/checkout@v4`**:
This step uses [actions/checkout](https://github.com/actions/checkout) to check out the repository code 
so that the workflow can access it.  Without this step, the code inside the repo that runs this action 
will not have a copy of the code.

#### 2. Run the Script (No Output)
```yaml
      - name: Run the script, no output.
        run: scripts/runme.sh 'an-argument'
```
- **`name`**: A descriptive name for the step.
- **`run: scripts/runme.sh 'an-argument'`**: Executes the `runme.sh` script located in the `scripts` directory, passing `'an-argument'` as an argument. The script runs, but its output is not captured.

#### 3. Run the Script and store the result in `GITHUB_OUTPUT`
```yaml
      - name: run script with Github Output.  Must be key=value pairs.
        id: script_results
        run: echo "results=$(scripts/runme.sh 'to-output')" >> $GITHUB_OUTPUT
```
- **`name`**: A descriptive name for the step.
- **`id: script_results`**: Assigns an ID to this step so that its output can be referenced later.
- **`run`**: Runs the `runme.sh` script with the argument `'to-output'` and captures its output in the `results` key. The output is stored in `$GITHUB_OUTPUT` for use in subsequent steps.

GITHUB_OUTPUT is somewhat of a *magic* variable that has special uses and meaning in the context of a 
GitHub action.  It will update the `steps` context object with `steps.<step-id>.outputs.<variable-name>`. 
Therefore, this will place the script output in the variable `steps.script_results.outputs.results`.

#### 4. Adding to `GITHUB_STEP_SUMMARY`
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

## Next Section
[back](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts) | 
[03-prebuilt-actions](https://github.com/BlueBastion/DEV-github-actions-example/tree/03-prebuilt-actions)

