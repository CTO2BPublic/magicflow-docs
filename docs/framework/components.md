# Components

## Events

### General Rules for Message Schema Formatting

- Use `underscore_case` for attribute names.
- Use lowercase for attribute names, except when referencing an object — in that case, use CapitalCase for the key.
  - Example: `"attribute": "some_string"` vs `"Attribute": {}`

### Standard Event Object Structure

Each event object must include:

- `id`: Unique identifier of the message.
- `Attributes`: Object containing header-like metadata:
  - `source`: The origin of the event (e.g., service name)
  - `type`: Event type for identification
  - `date`: Timestamp of the event
- `Data`: Contains parameters intended for the event receiver (e.g., result of a command, or input parameters for a worker)
- `message`: _(Optional)_ Human-readable message for debugging or clarity

---

## Command

A **command** is an object placed into a queue system to instruct the worker to perform an action.

### Supported Commands

| Command | Description | Schema Example |
|---------|-------------|----------------|
| `RunJob` | Executes the specified job using the worker engine | 
```json
{
  "id": "xxx-x-x-x-xxx-x-x-x-xx",
  "Attributes": {
    "source": "magicflow-controller",
    "type": "RunJob",
    "date": "2025-05-15T09:55:28.81Z"
  },
  "Data": {
    "name": "create_check_namespace",
    "workflow_id": "xxx-xxxx-xxxx",
    "Input": {
      "namespace_name": "poc-db"
    },
    "Metrics": "<MetricsObject>"
  }
}
``` 


---

## Events

| Event | Description | Schema Example |
|-------|-------------|----------------|
| `RunJobCompleted` | Indicates successful job completion | |
```json
{
  "id": "xxx-x-x-x-xxx-x-x-x-xx",
  "Attributes": {
    "source": "magicflow-worker",
    "type": "RunJobCompleted",
    "date": "2025-05-15T09:55:28.81Z"
  },
  "message": "RunJobCompleted completed successfully",
  "Data": {
    "name": "create_check_namespace",
    "workflow_id": "xxxx-xxxx-xxx",
    "Input": "<CommandObject>",
    "Output": "<OutputObject>",
    "Metrics": "<MetricsObject>"
  }
}
``` 
 `RunJobFailed`  Indicates job failure 
```json
{
  "id": "xxx-x-x-x-xxx-x-x-x-xx",
  "attributes": {
    "source": "magicflow-worker",
    "type": "RunJobFailed",
    "date": "2025-05-15T09:55:28.81Z"
  },
  "message": "Something went wrong",
  "Data": {
    "name": "create_check_namespace",
    "workflow_id": "xxxx-xxxx-xxx",
    "Input": "<CommandObject>",
    "error": "SomeError",
    "Stack": "<StackTrace>",
    "status": "500",
    "Metrics": "<MetricsObject>"
  }
}
``` 
 `WorkflowStarted`  Indicates the start of a workflow  
 ```json
{
  "id": "xxx-x-x-x-xxx-x-x-x-xx",
  "attributes": {
    "source": "magicflow-controller",
    "type": "WorkflowStarted",
    "date": "2025-05-15T09:55:28.81Z"
  },
  "message": "Started workflow",
  "Data": {
    "workflow_id": "xxxx-xxxx-xxx",
    "Metrics": "<MetricsObject>"
  }
}
``` 
 `WorkflowCompleted`  Indicates successful completion of a workflow 
  ```json
{
  "id": "xxx-x-x-x-xxx-x-x-x-xx",
  "attributes": {
    "source": "magicflow-controller",
    "type": "WorkflowCompleted",
    "date": "2025-05-15T09:55:28.81Z"
  },
  "message": "Workflow completed",
  "Data": {
    "workflow_id": "xxxx-xxxx-xxx",
    "Metrics": "<MetricsObject>",
    "Output": "<OutputObject>"
  }
}
``` 
 `WorkflowFailed`  Indicates a workflow has failed 
  ```json
{
  "id": "xxx-x-x-x-xxx-x-x-x-xx",
  "attributes": {
    "source": "magicflow-controller",
    "type": "WorkflowFailed",
    "date": "2025-05-15T09:55:28.81Z"
  },
  "message": "Something went wrong",
  "Data": {
    "workflow_id": "xxxx-xxxx-xxx",
    "error": "SomeError",
    "Stack": "<StackTrace>",
    "status": "500",
    "Metrics": "<MetricsObject>"
  }
}
``` 

---

## Event Status

### RunJobFailed

| Status Code | Description |
|-------------|-------------|
| 501         | Validation error |
| 500         | Internal error   |

### WorkflowFailed

| Status Code | Description      |
|-------------|------------------|
| 400         | Not authorized   |
| 500         | Internal error   |

---

## MetricsObject

The **MetricsObject** is used to append service-related timestamp checkpoints for calculating in-service and end-to-end (E2E) metrics. Each "step" in a process can push a checkpoint to the `MetricsObject`, enabling complete traceability.

| Stack Position (Index) | Checkpoint Name             | Timestamp (s) |
|------------------------|-----------------------------|---------------|
| 0                      | api_workflow_requested      | 100           |
| 1                      | controller_command_sent     | 101           |
| 2                      | worker_message_received     | 110           |
| 3                      | worker_started_job          | 111           |
| 4                      | worker_job_completed        | 119           |
| 5                      | controller_event_received   | 125           |
| 6                      | controller_workflow_completed | 130         |

From this stack, metrics like E2E duration can be calculated:  
`controller_workflow_completed - api_workflow_requested = 30s`
