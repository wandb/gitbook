---
description: How to upgrade the W&B Enterprise Server installation
---

# Upgrades

Each W&B Enterprise Server comes equipped with the ability to self-update with new releases of our software. You'll be notified when new releases are available, and the upgrade process is available on the `/vm-settings` page on your VM.

We will infrequently release new W&B VM versions which require a full replacement of the instance. Instructions for how to manage this process follow.

### Amazon Web Services

To seamlessly migrate your data from one W&B Enterprise Server to a new one on AWS, follow the following steps.

1. In the "EC2 &gt; Instances" section of the AWS Console, find your existing W&B server and select "Actions &gt; Instance State &gt; Stop".
2. In the Instance Description area, find and click on "Block Devices &gt; /dev/sdc &gt; EBS ID" to navigate to the volume containing your server's data.
3. Select "Actions &gt; Create Snapshot". Give your snapshot a memorable description and name \("click to add a Name tag"\).
4. When the snapshot is created, copy the snapshot ID.
5. Navigate to "EC2 &gt; AMIs" in the AWS Console and select the newly released W&B AMI that was shared with your account. Click "Launch".
6. Proceed through the instance launch process as usual. In Step 4: Add Storage, for the second volume \(/dev/sdc\), paste your snapshot ID into the Snapshot column. You may also increase the size of your volume at this time.
7. Continue to launch your instance as usual. When the instance comes live, it will contain the data of your previous W&B Server.
8. You may now terminate the previous W&B Server Instance.

