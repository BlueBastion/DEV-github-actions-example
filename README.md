# Github Actions Intro

## Guide  
Intro steps are made in branches to develop a new idea, one at a time.

- 03: [Prev](https://github.com/BlueBastion/DEV-github-actions-example/tree/03-prebuilt-actions)

# Build Artifact Handling
In this section we've added [upload-build-artifacts.yml](https://github.com/BlueBastion/DEV-github-actions-example/blob/04-build-artifacts/.github/workflows/upload-build-artifacts.yml) 
and removed the other ones for cleanliness.  This workflow demonstrates how to preserve artifacts or 
results created during a job run.

## Key Concepts
#### Build Artifacts
Any generated files and or build results may be uploaded and attached to the run.
They can be retreived in any subsequent jobs.  If you would like to download them in another job, 
you must have the run-id of the job that the artifact was created in.

#### Releases
Releases are a type of build artifact that we saw in the last section.

## Actions Used
##### [*`actions/upload-artifact`*](https://github.com/marketplace/actions/upload-a-build-artifact)
##### [*`actions/download-artifact`*](https://github.com/marketplace/actions/download-a-build-artifact)

## Overview
As usual we're setting up the triggers and jobs in a similar way to what we've seen before.

We'll create two text files, upload those files and then create a step summary to display when the job is complete.

