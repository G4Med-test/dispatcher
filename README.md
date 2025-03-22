# Dispatcher Repository

This repository serves as a job queue for distributed execution of G4Med-test projects.

## How It Works
- Each job is described in a JSON file inside the `jobs/` folder.
- Agents running on external servers periodically poll this repository, claim available jobs via GitHub Issues, and execute them.
- The first agent to claim an issue runs the job, posts results, and closes the issue.

## Job JSON structure
| Field             | Description                                                         |
|-------------------|---------------------------------------------------------------------|
| job_id            | Unique identifier for the job.                                     |
| repository        | The GitHub repository containing the source code.                  |
| commit            | Commit hash or branch name to clone.                               |
| container_image   | URL of the pre-built Apptainer container on GHCR.                  |
| input_folder      | Relative folder containing input macro files.                      |
| output_folder     | Relative folder where output will be stored.                       |
| priority          | Priority level (1 = highest).                                      |
| assigned_server   | Preferred server to run the job (or `any`).                        |
| timestamp         | Date the job was created.                                          |
| retry_count       | Counter for how many times the job has been retried.               |
| notes             | Additional notes.                                                  |

## Validation Workflow
A GitHub action validates each job JSON file upon PR submission.

## Issue-based queue
- Each job JSON triggers an Issue creation (automated or manually).
- Agents claim jobs by commenting on the issue.
- Upon completion, agents close the issue with links to output results and release.# dispatcher
