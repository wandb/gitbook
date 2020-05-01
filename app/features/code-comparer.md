# Code Comparer

Experimenting with a machine learning model means making lots of quick changes to the code. If you’re training in a git repository, we’ve always captured the latest commit and a patch file containing any uncommitted changes. Today, I’m excited to announce two new features that will help you see exactly what code changed between machine learning experiments: the Code Comparer and Jupyter Session History.

### Code Comparer

Starting with **wandb** version 0.8.28, our library now saves the code from your main training file where you call **wandb.init\(\)**. When you add a new custom panel to your workspace or report, you’ll now find Code Comparer as an option. Diff any two experiments in your project and see exactly which lines of code changed. Here’s an example:

![](../../.gitbook/assets/cc1.png)

### Jupyter Session History

Starting with **wandb** version 0.8.34, our library does Jupyter session saving. When you call **wandb.init\(\)** inside of Jupyter, we add a hook to automatically save a Jupyter notebook containing the history of code executed in your current session. You can find this session history in a runs file browser under the code directory:

![](../../.gitbook/assets/cc2%20%281%29.png)

Clicking on this file will display the cells that were executed in your session along with any outputs created by calling iPython’s display method. This enables you to see exactly what code was run within Jupyter in a given run. When possible we also save the most recent version of the notebook which you would find in the code directory as well.

![](../../.gitbook/assets/cc3.png)

### Jupyter diffing

One last bonus feature is the ability to diff notebooks. Instead of showing the raw JSON in our Code Comparer panel, we extract each cell and display any lines that changed. We have some exciting features planned for integrating Jupyter deeper in our platform.

