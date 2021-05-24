# Reproducible Deep Learning
## Exercise 1: Git & Scripting
[[Slides](https://docs.google.com/presentation/d/1_AYIcCyVI59QiiXqU4Sn7VzwtVyfqv-lG36EPFzeSdY/edit?usp=sharing)]

## Objectives for the exercise

- [x] Experimenting with Git branches.
- [x] Turning a notebook into a runnable script.


1. Start by initializing and moving to an experimental branch:

```bash
git branch experimental_branch
git checkout experimental_branch
```

2. Convert the notebook into a Python script by running `nbconvert`:

```bash
jupyter nbconvert --to script --output "train" "Initial Notebook.ipynb"
```

=======

The repository contains the code for the different tools that I learned from the course [[Official website](https://www.sscardapane.it/teaching/reproducibledl/)]. 

I would like to thank [Prof. Simone Scardapane](https://www.sscardapane.it/) for providing the password for accessing the video lectures. 

This practical course helped me in exploring the design of a simple *reproducible* environment for a deep learning project. I learned following tools and frameworks while doing the exercises:
- [x]  [DVC](http://dvc.org/) for data versioning
- [x] [Docker](https://www.docker.com/) for setting up environments 
- [x] [Hydra] (https://github.com/facebookresearch/hydra) for logging hyperparameters
- [x] [Wandb] (https://wandb.ai/site) Weights and Biases for ML Experimentation Management and Hyperparameter Sweeping
- [x] [Github Actions] (https://github.com/features/actions) Github Actions for Continous Integration



<p align="center">
<img align="center" src="https://github.com/shub-kris/reprodl2021/blob/main/reprodl_overview.png" width="500" style="border: 1px solid black;">
</p>

3. Reorganize the script so that it is runnable from terminal:
   * Remove all instructions that are not required for training;
   * Put all training instructions inside a new `train()` function.

4. Add a [top-level instruction](https://docs.python.org/3/library/__main__.html) to run the module as a script:

```python
if __name__ == "__main__":
    train()
```

5. Create a [.gitignore file](https://git-scm.com/docs/gitignore) to ignore the *data* and *lightning_logs* folders.
6. Remove the notebook, and check that the training script is working correctly:

```bash
python train.py
```

7. Merge the experimental branch into the main one, and delete the experimental branch:

```bash
git checkout main
git merge experimental_branch
git branch -d experimental_branch
```
