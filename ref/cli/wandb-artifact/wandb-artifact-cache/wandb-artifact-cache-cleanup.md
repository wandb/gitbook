# wandb artifact cache cleanup

**Usage**

`wandb artifact cache cleanup [OPTIONS] TARGET_SIZE`

**Summary**

Clean up less frequently used files from the artifacts cache. This will allow you to reclaim space from the artifacts cache and will purge files until we are under the `TARGET_SIZE`. For example: `wandb artifact cache cleanup 4GB`.

**Options**

| **Option** | **Description**             |
| ---------- | --------------------------- |
| --help     | Show this message and exit. |
