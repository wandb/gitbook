---
description: >-
  Manage a team's members, avatar, alerts, and privacy settings with the Team
  Settings page.
---

# Team settings

Navigate to your team’s profile page and select the **Team settings** icon to manage team settings. Not all members in a team can modify team settings. The account type (Administrator, Member, or Service) of a member determines what settings that member can modify. For example, only Administration account types can change team privacy settings and remove a member from a team.

### Members

The **Members** section shows a list of all pending invitations and the members that have either accepted the invitation to join the team. Each member listed displays a member’s name, username, and account type. There are three account types: Administrator (Admin), Member, and Service.

#### Change a member's role in the team

Complete the proceeding steps to change a member's role in a team:

1. Select the account type icon next to the name of a given team member. A modal will appear.
2. Select the drop-down menu.
3. From the drop-down, choose the account type you want that team member to posses.

#### Remove a member from a team

Select the trash can icon next to the name of the member you want to remove from the team.

{% hint style="info" %}
Runs created in a team account are preserved when the member who created those runs are removed from the team.
{% endhint %}

### Avatar

Set an avatar by navigating to the **Avatar** section and uploading an image.&#x20;

1. Select the **Update Avatar** to prompt a file dialog to appear.&#x20;
2. From the file dialog, choose the image you want to use.

### Alerts

Notify your team when runs crash, finish, or set custom alerts. Your team can receive alerts either through email or Slack.

Toggle the switch next to the event type you want to receive alerts from. Weights and Biases provides the following event type options be default:

* **Runs finished**: whether a Weights and Biases run successfully finished.
* **Run crashed**: if a run has failed to finish.

For more information about how to set up and manage alerts, see [Send alerts with wandb.alert](https://docs.wandb.ai/guides/track/alert).

### Privacy

Navigate to the **Privacy** section to change privacy settings. Only members with Administrative roles can modify privacy settings. Administrator roles can:

* Force projects in the team to be private.
* Enable code saving by default.

### Usage

The **Usage** section describes the total memory usage the team has consumed on the Weights and Biases servers. The default storage plan is 100GB. For more information about storage and pricing, see the [Pricing](https://wandb.ai/site/pricing) page.&#x20;
