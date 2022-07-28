---
description: >-
  Manage a team's members, avatar, alerts, and privacy settings with the Team
  Settings page.
---

# Team settings

Navigate to your team’s profile page and select the **Team settings** icon to manage your team's settings. Not all members in a team can modify the team's settings.  The account type (Administrator, Member, or Service) of a member determines what settings that member can modify. For example, only Administration account types can change team privacy settings and remove a member from a team.

### Members

The **Members** section shows a list of all pending invitations and the members that have either accepted the invitation to join the team. Each member listed displays the member’s name, username, and account type. There are three account types: Administrator (Admin), Member, and Service.

#### Change a member's role in the team

Select the account type icon next to the name of a given team member. A modal will appear. Select the drop-down menu and choose the account type you want that team member to posses.

#### Remove a member from a team

Select the trash can icon next to the name of the member you want to remove from the team.

### Avatar

Set a team's avatar by navigating to the **Avatar** section and uploading an image. Select the **Update Avatar** to prompt a file dialog to appear. From the file dialog, choose the image you want to use the team's avatar.

### Alerts

Notify the team’s Slack workspace when a member’s run finishes or crashes. There are two types of alerts your team can receive by email or Slack:

* **Runs finished**: whether a Weights and Biases run successfully finished.
* **Run crashed**: notification if a run has failed to finish.

For more information about setting up alerts for Weights and Biases runs, see [Send alerts with wandb.alert](https://docs.wandb.ai/guides/track/alert).

### Privacy

Navigate to the **Privacy** section to change privacy settings. Only members with Administrative roles can modify privacy settings. Administrator roles can modify:

* Force projects in the team to be private.
* Enable code saving by default.

### Usage

The **Usage** section describes the total memory usage the team has consumed on the Weights and Biases servers. The default storage plan is 100GB. For more information about storage and pricing, see the [Pricing](https://wandb.ai/site/pricing) page.&#x20;
