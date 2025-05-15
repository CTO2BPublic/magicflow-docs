# Job examples

- [Gitlab Pipeline](./pipepline.md) - work with Gitlab pipeline
  - job: "check_pipeline_status" - Monitors the status of a GitLab merge request pipeline, pausing the workflow if the pipeline is still running or waiting, and returning the final status when complete.
  - job: "check_pipeline_job_status" - Tracks the status of specific jobs within a GitLab pipeline, pausing the workflow if jobs are still running, and returning the final job status when complete.
  - job: "play_pipeline_job" - Triggers a manual job in a GitLab pipeline and returns the job details and status after execution.
 - [Secret Manager](./ssm.md) - work with secret manager GCP and AWS
    - job: "ssm_list_all" - Lists all parameters/secrets from AWS SSM Parameter Store or GCP Secret Manager for a given cloud, stage, and environment.
    - job: "ssm_list" - Retrieves specific parameters/secrets from AWS SSM Parameter Store or GCP Secret Manager based on namespace and name.
    - job: "ssm_update" - Updates existing parameters/secrets in AWS SSM Parameter Store or GCP Secret Manager with new values provided in YAML format.