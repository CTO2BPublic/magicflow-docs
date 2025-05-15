# Gitlab Pipeline Example

Place file in jobs lib with all required jobs(functions with decorator @). All jobs will be loaded on worker pod start 

```
magicflow/
└── jobs/
    └── lib/
        ├── dummy.py
        └── ssm.py
        └── pipeline.py <---
```

```python
import time
from magicflow.messaging import CommandMessage
from magicflow.libs.gitlab_service import GitlabDriver
from magicflow.libs.logging_service import LoggingService
from magicflow.config.config import settings
from magicflow.jobs import jobs, Jobs

logger = LoggingService().getLogger(__name__)
j = jobs()

@j.register("check_pipeline_status")
def check_pipeline_status(context: Jobs, cmd: CommandMessage):
    try:
        input = cmd.get_data()["input"]
        if input["mr_created"] == "False" or input[
                "expect_pipeline"] == "False":
            return {"result": "No pipelines to check"}

        assert input["mr_id"]
        mr_id = input["mr_id"]
        assert input["project_id"]
        project_id = input["project_id"]
        expect_pipeline = input["expect_pipeline"]

        gl = GitlabDriver(settings.gitlab_api_token, project_id)
        time.sleep(10)
        pipelines = gl.check_mr_pipeline_status(mr_id)
        logger.debug(f' Pipeline status: {pipelines}')
        if pipelines:
            for p in pipelines:
                # The status of pipelines, one of:
                # created, waiting_for_resource, preparing, pending,
                # running, success, failed, canceled, skipped, manual, scheduled
                # ref: https://docs.gitlab.com/ee/api/pipelines.html
                pid = p["id"]
                purl = p["url"]
                pstatus = p["status"]
                if p["status"] in [
                        "created", "waiting_for_resource", "preparing",
                        "pending", "running"
                ]:
                    return {
                        "result": f'Pipeline [#{pid}]({purl}) {pstatus}',
                        "pipeline": p,
                        "workflow_control": {
                            "pause": "true",
                            "pause_seconds": 10
                        }
                    }
                else:
                    return {
                        "result": f'Pipeline [#{pid}]({purl}) {pstatus}',
                        "parameters": {
                            "pipeline_id": str(p['id']),
                            "project_id": project_id
                        },
                        "pipeline": p
                    }
        else:
            if expect_pipeline == "True":
                return {
                    "result": f'Pipeline expected but not scheduled ',
                    "workflow_control": {
                        "pause": "true",
                        "pause_seconds": 20
                    }
                }
            else:
                return {"result": f' No Pipelines expected'}
    except KeyError:
        logger.warning('Missing input parameters')
        return {"result": f' No Pipelines found'}


@j.register("check_pipeline_job_status")
def check_pipeline_job_status(context: Jobs, cmd: CommandMessage):
    try:
        input = cmd.get_data()["input"]
        assert input["pipeline_job_id"]
        pipeline_job_id = input["pipeline_job_id"]
        assert input["project_id"]
        project_id = input["project_id"]
        assert input["pipeline_id"]
        pipeline_id = input["pipeline_id"]

        gl = GitlabDriver(settings.gitlab_api_token, project_id)
        time.sleep(10)
        jobs = gl.check_pipeline_job_status(pipeline_job_id)
        logger.debug(f' JOBS status: {jobs}')
        if jobs:
            for _job in jobs:
                logger.debug(f' JOB status: {_job}')
                jid = _job["id"]
                jurl = _job["url"]
                jstatus = _job["status"]
                if _job["status"] in [
                        "created", "waiting_for_resource", "preparing",
                        "pending", "running"
                ]:
                    return {
                        "result": f'Job [#{jid}]({jurl}) {jstatus}',
                        "pipeline_job": _job,
                        "workflow_control": {
                            "pause": "true",
                            "pause_seconds": 10
                        }
                    }
                else:
                    return {
                        "result": f'Job [#{jid}]({jurl}) {jstatus}',
                        "parameters": {
                            "pipeline_id": pipeline_id,
                            "pipeline_job_id": str(_job['id']),
                            "project_id": project_id
                        },
                        "pipeline_job": _job
                    }
        else:
            return {"result": f' No Jobs found'}
    except KeyError:
        logger.error('Missing input parameters')
        return {"result": f' No Job found'}


@j.register("play_pipeline_job")
def play_pipeline_job(context: Jobs, cmd: CommandMessage):
    try:
        input = cmd.get_data()["input"]
        assert input["project_id"]
        project_id = input["project_id"]
        assert input["pipeline_id"]
        pipeline_id = input["pipeline_id"]
        gl = GitlabDriver(settings.gitlab_api_token, project_id)
        time.sleep(10)
        play_job = gl.play_pipeline_job(pipeline_id)
        if play_job:
            play_job.update({"project_id": project_id})
            if play_job["pipeline_job_id"]:
                return {
                    "parameters":
                    play_job,
                    "result":
                    f'Job [#{play_job["pipeline_job_id"]}]({play_job["web_url"]}) played'
                }
            else:
                return {"parameters": play_job, "result": 'Failed to play job'}
        else:
            return {"result": f' No Pipelines found'}
    except AssertionError:
        logger.error('Missing input parameters')
        return {"result": f' No Jobs found'}
    except KeyError:
        return {"result": f' No Jobs to check found'}
```

example workflow to import in to workflow controller. Construct workflow from multiple jobs, where you can pass one job output to another job input by reference it 

```
            "name": "check_pipeline_status",
            "order": 6,
            "input": {
                "from_jobs": [1],
                "expect_pipeline": "True"
            },
```
as per above example you pass to job order 6 inputs from 1st job output and additionaly adding extra input parameter `expect_pipeline`

```json
{
    "name": "Work With Gitlab Example",
    "template": "true",
    "description": "Provision resource in new environment",
    "category": "Provisioning",
    "icon": "app.png",
    "type": "Some service",
    "parameters": [
        {
            "Name": "stage",
            "value": "dev",
            "description": "Stage name (stage)"
        },
        {
            "Name": "environment",
            "value": "development",
            "description": "Environment name (environment)"
        },
        {
            "Name": "namespace",
            "value": "test",
            "description": "Name of the namespace on which a new service will be provisioned (namespace)"
        },
        {
            "Name": "instance",
            "value": "mysuperapp",
            "description": "Service instance name (instance)"
        },
        {
            "Name": "cloud_provider",
            "value": "gcp",
            "description": "Cloud provider (cloud_provider)"
        },
        {
            "Name": "jira_ticket",
            "value": "SD-XXXX",
            "description": "JIRA ticket number which to add to commit messages"
        }
    ],
    "jobs": [
        {
            "name": "check_environment_exists",
            "order": 1,
            "input": {
                "stage": "",
                "environment": "",
                "namespace": "",
                "instance": ""
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },
        {
            "name": "create_namespace",
            "order": 2,
            "input": {
                "stage": "",
                "environment": "",
                "namespace": "",
                "service": "someapp",
                "instance": "",
                "jira_ticket": ""
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },
        {
            "name": "check_pipeline_status",
            "order": 3,
            "input": {
                "from_jobs": [1],
                "namespace": "",
                 "expect_pipeline": "True"
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },
        {
            "name": "check_approval",
            "order": 4,
            "input": {
                "from_jobs": [1]
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },
        {
            "name": "create_mr_nonprod",
            "order": 5,
            "input": {
                "stage": "",
                "environment": "",
                "namespace": "",
                "service": "someapp",
                "instance": "",
                "cloud_provider": "",
                "jira_ticket": ""
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },
        {
            "name": "check_pipeline_status",
            "order": 6,
            "input": {
                "from_jobs": [4],
                 "expect_pipeline": "True"
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },
        {
            "name": "check_approval",
            "order": 7,
            "input": {
                "from_jobs": [4]
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },     
        {
            "name": "merge",
            "order": 8,
            "input": {
                "from_jobs": [1]
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },
        {
            "name": "merge",
            "order": 9,
            "input": {
                "from_jobs": [4]
            },
            "output": {
                "error": {}
            },
            "status": "New"
        },           
    ]
}
```

