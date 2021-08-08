# wandb agent

**Usage**

`wandb agent [OPTIONS] SWEEP_ID`

**Summary**

Run the W&B agent for a specified sweep, drawing entity and project information from the `SWEEP_ID`, `agent` flags, or client settings.

`SWEEP_ID`  can be formatted in one of three ways: `sweep_name`, `project_name/sweep` or `entity_name/project_name/sweep_name`. If entity and project information are provided as sections of the `SWEEP_ID`, these values will take precedence over any values specified to the `-e` and `-p` flags. If no entity or project information is specified, the default entity and project associated with the current API key will be used to identify the sweep. 

**Options**

| **Option** | **Description** |
| :--- | :--- |
| -p, --project | The project of the sweep. |
| -e, --entity | The entity scope for the project. |
| --count | The max number of runs for this agent. |
| --help | Show this message and exit. |

