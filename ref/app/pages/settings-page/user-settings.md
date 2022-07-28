---
description: >-
  Manage your profile information, account defaults, alerts, participation in
  beta products, GitHub integration, storage usage, account activation, and
  create teams in your user settings.
---

# User settings

Navigate to your user profile page and select your user icon on the top right corner. From the dropdown, choose **Settings**.&#x20;

### Profile

Within the **Profile** section you can manage and modify your account name and institution. You can optionally add a biography, location, link to a personal or your institution’s website, and upload a profile image.

### Project defaults

Change the default behavior for your account within the **Project** **Defaults** section. You can manage the proceeding:

* **Default location to create new projects** - Select the dropdown menu and choose the entity to set as the new default. Specify either your account or a team you are a member of.
* **Default projects privacy in your personal account** - Set a project to public (anyone can view), private (only you can view and contribute) or open (anyone can submit runs or write the repots) automatically when you create a project. You can optionally create a team to collaborate on private projects.
* **Enable code savings in your personal account** - Permit Weights and Biases to save the latest git commit hash by default. To enable code saving, toggle the Enable code savings in your personal account option. For more information about saving and comparing code, see [Code Saving](https://docs.wandb.ai/ref/app/features/panels/code).

### Teams

Create a new team in the **Team** section. To create a new team, select the **New team** button and provide the following:

* **Team name** - the name of your team. The team mane must be unique. Team names can not be changed.
* **Team type** - Select either the **Work** or **Academic** button.
* **Company/Organization** - Provide the name of the team’s company or organization. Choose the dropdown menu to select a company or organization. You can optionally provide a new organization.

{% hint style="info" %}
Only administrative accounts can create a team.
{% endhint %}

### Beta features

Within the **Beta Features** section you can optionally enable fun add-ons and sneak previews of new products in development. Select the toggle switch next to the beta feature you want to enable. Currently you can get a sneak peak of the proceeding features:

* **Night mode** - Invert the colors everywhere! This makes pages dark, but might make colors on charts less easy to distinguish.
* **W\&B Launch** - Launch jobs on your own infrastructure from Weights and Biases. For more information, see the [Launch Jobs](https://docs.wandb.ai/guides/launch?\_gl=1\*pdlnmj\*\_ga\*MjQzNTM2NTgwLjE2NTQwMTQ1NzA.\*\_ga\_JH1SJHJQXJ\*MTY1OTAzMTAwMS45Ni4xLjE2NTkwMzM5NjYuNjA.) documentation.&#x20;
* **Unicorn mode** - Change your cursor on charts from a boring pointer to a unicorn!

### Alerts

Get notified when your runs crash, finish, or set custom alerts with [wandb.alert()](https://docs.wandb.ai/guides/track/alert). Receive notifications either through Email or Slack. Toggle the switch next to the event type you want to receive alerts from.

* **Runs finished**: whether a Weights and Biases run successfully finished.
* **Run crashed**: notification if a run has failed to finish.

For more information about how to set up and manage alerts, see [Send alerts with wandb.alert](https://docs.wandb.ai/guides/track/alert).

### Personal GitHub integration

Connect a personal Github account. To connect a Github account:

1. Select the **Connect Github** button. This will redirect you to an open authorization (OAuth) page.
2. Select the organization to grant access in the **Organization access** section.
3. Select **Authorize** **wandb**.

### Delete your account

Select the **Delete Account** button to delete your account.

{% hint style="danger" %}
Account deletion can not be reversed.
{% endhint %}

### Storage

The **Storage** section describes the total memory usage the your account has consumed on the Weights and Biases servers. The default storage plan is 100GB. For more information about storage and pricing, see the [Pricing](https://wandb.ai/site/pricing) page.
