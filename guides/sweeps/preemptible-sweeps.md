---
description: Supported as of version 0.10.31
---

# Preemptible Sweeps

If you are running a sweep agent in a compute environment that is subject to preemption  \(e.g., a SLURM job in a preemptible queue, an EC2 spot instance, or a Google Cloud preemptible VM\), you can automatically requeue your interrupted sweep runs, ensuring they will be retried until they run to completion.

#### Handling Preemption

If you plan to run a sweep agent on a preemptible compute instance and would like to take advantage of automatic requeueing for preempted runs, first implement a method that can be called to query the instance's preemption status. Most cloud providers provide a metadata server that can be queried to get preemption notices. 

* AWS EC2 Spot Instance Termination Notices: [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html\#spot-instance-termination-notices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html#spot-instance-termination-notices)
* Google Preemptible VM Preemption Signals: [https://cloud.google.com/compute/docs/instances/preemptible\#preemption-process](https://cloud.google.com/compute/docs/instances/preemptible#preemption-process)
* slurm preemption: [https://slurm.schedmd.com/preempt.html](https://slurm.schedmd.com/preempt.html#:~:text=Slurm%20supports%20job%20preemption%2C%20the,of%20Slurm's%20Gang%20Scheduling%20logic.)

Once you have a method that you can call to determine whether you are about to be preempted, query it periodically during your training script. When you learn your current run is about to be preempted, call 

```
wandb.mark_preempting()
```

to immediately signal to the W&B backend that your run believes it is about to be preempted. 

#### When is a run requeued?

If a run that is marked preeempting exits with status code 0, W&B will consider the run to have terminated successfully and it will not be requeued. If a preempting run exits with a nonzero status, W&B will consider the run to have been preempted, and it will automatically append the run to a run queue associated with the sweep. If a run exits with no status, W&B will mark the run preempted 5 minutes after the run's final heartbeat, then add it to the sweep run queue.

#### In what order are requeued runs run?

Sweep agents will consume runs off the run queue until the queue is exhausted, at which point they will resume generating new runs based on the standard sweep search algorithm. 

#### Do requeued runs resume running at the part of training where they were interrupted?

By default, requeued runs begin logging from their initial step. To instruct a run to resume logging at the step where it was interrupted, initialize the resumed run with `wandb.init(resume='allow')`.

