---
description: Send alerts, triggered from your Python code, to your Slack or email
---

# Send Alerts with wandb.alert

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://wandb.me/alerts)

With W\&B Alerts you can be notified via Slack or email if your W\&B Run has crashed or whether a custom trigger, such as your loss going to NaN or a step in your ML pipeline has completed, has been reached. W\&B Alerts apply all projects where you launch runs, including both personal and Team projects.

You can set an alert like this:

```python
wandb.alert(
    title="Low accuracy", 
    text=f"Accuracy {acc} is below the acceptable threshold {thresh}"
)
```

And then see W\&B Alerts messages in Slack (or your email):

![](<../../../.gitbook/assets/Screenshot 2022-02-17 at 16.26.15 (1).png>)

## Getting Started

There are 2 steps to follow the first time you'd like to send a Slack or email alert, triggered from your code:

1. Turn on Alerts in your W\&B [User Settings](https://wandb.ai/settings)
2. Add `wandb.alert()` to your code

### 1. Turn on Alerts in your W\&B User Settings

In your [User Settings](https://wandb.ai/settings):

* Scroll to the **Alerts** section
* Turn on **Scriptable run alerts** to receive alerts from `wandb.alert()`
* Use **Connect Slack** to pick a Slack channel to post alerts. We recommend the **Slackbot** channel because it keeps the alerts private.
* **Email** will go to the email address you used when you signed up for W\&B. We recommend setting up a filter in your email so all these alerts go into a folder and don't fill up your inbox.

You will only have to do this the first time you set up W\&B Alerts, or when you'd like to modify how you receive alerts.

![Alerts settings in W\&B User Settings](<../../../.gitbook/assets/demo - connect slack.png>)

### 2. Add \`wandb.alert()\` to Your Code

Add `wandb.alert()` to your code (either in a Notebook or Python script) wherever you'd like it to be triggered

```python
wandb.alert(
    title="High Loss", 
    text="Loss is increasing rapidly"
)
```

#### Check your Slack or email

Check your Slack or emails for the alert message. If you didn't receive any, make sure you've got emails or Slack turned on for **Scriptable Alerts** in your **** [User Settings](https://wandb.ai/settings)

## Using \`wandb.alert()\`



| Argument                   | Description                                                                                                                                           |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `title` (string)           | A short description of the alert, for example "Low accuracy"                                                                                          |
| `text` (string)            | A longer, more detailed description of what happened to trigger the alert                                                                             |
| `level` (optional)         | How important the alert is — must be either `AlertLevel.INFO`, `AlertLevel.WARN`, or `AlertLevel.ERROR.` You can import `AlertLevel.xxx` from `wandb` |
|                            |                                                                                                                                                       |
| `wait_duration` (optional) | How many seconds to wait before sending another alert with the same **title.** This helps reduce alert spam                                           |

### Example

This simple alert sends a warning when accuracy falls below a threshold. In this example, it only sends alerts at least 5 minutes apart.

[Run the code →](http://wandb.me/alerts)

```python
import wandb
from wandb import AlertLevel

if acc < threshold:
    wandb.alert(
        title="Low accuracy", 
        text=f"Accuracy {acc} is below the acceptable theshold {threshold}",
        level=AlertLevel.WARN,
        wait_duration=300
    )
```

## More Info

### W\&B Team Alerts

Team admins can set up alerts for the team on the team settings page: wandb.ai/teams/`your-team`. These alerts apply to everyone on your team. We recommend using the **Slackbot** channel because it keeps the alerts private.

### Changing Slack Channels

To change what channel you're posting to, click **Disconnect Slack** and then reconnect, picking a different destination channel.

### Note

"**Run Finished" Alerts in Jupyter**

{% hint style="warning" %}
Note that "**Run Finished"** alerts (turned on via the "**Run Finished"** setting in User Settings) only work with Python scripts and **** are disabled in Jupyter Notebook environments to prevent alert notifications on every cell execution. Use `wandb.alert()` in Jupyter Notebook environments instead.
{% endhint %}

**Alerts with W\&B Local**

{% hint style="warning" %}
Note that if you are self-hosting using W\&B Local you will need to follow [these steps](../../self-hosted/configuration.md#slack) before enabling Slack alerts
{% endhint %}
