---
description: Collaborate with your colleagues using our private teams
---

# Teams

Use our private teams feature to collaborate on private projects. If you'd like to set up a private team, please message us at contact@wandb.com. 

## Common Questions

### Move runs to a team

It's easy to move runs between projects you have access to. On the project page:

1. Click the table tab to expand the runs table
2. Click the checkbox to select all runs
3. Click "Move" to select the destination project. This can be in your personal account or any team that you're a member of.

### Send new runs to a team

In your script, set the entity to your team. "Entity" just means your username or team name. Create an entity \(personal account or team account\) in the web app before sending runs there.

```python
wandb.init(entity="example-team")
```

### Invite team members

You can invite new members to your team on your team settings page:  
app.wandb.ai/teams/&lt;your-team-here&gt;

### See privacy settings

You can see the privacy settings of all team projects on the team settings page:  
app.wandb.ai/teams/&lt;your-team-here&gt;

Here's what the settings look like. In this screenshot the toggle is on, which means all projects in the team are only visible to the team.

![](../../.gitbook/assets/image%20%289%29.png)

