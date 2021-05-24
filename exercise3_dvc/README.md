# Reproducible Deep Learning
## Exercise 3: Data versioning with DVC
[[Official website](https://www.sscardapane.it/teaching/reproducibledl/)] [[Slides](https://docs.google.com/presentation/d/1jUFz212lZvwqDibiCRoOcm-40ANPXI1dKlF8t7PD1Is/edit?usp=sharing)] [[DVC Website](http://dvc.org/)]

## Objectives for the exercise

- [x] Adding data versioning with DVC.
- [x] Using multiple remotes for your DVC repository.
- [x] Downloading files from a DVC repository.


## Prerequisites

1. Install [DVC](http://dvc.org/):

```bash
pip install dvc
```

2. If plan on using S3 storage for exercise 3.2, install `boto3`:

```bash
pip install boto3
```

## Exercise 3.1 - Setup a DVC repository

For the first exercise, we setup versioning of the ESC-50 dataset, using a separate folder from the computer. Before starting, read the slides and the [DVC User Guide](https://dvc.org/doc/start/data-and-model-versioning).

1. Initialize the DVC repository. This creates a `.dvc` folder with the necessary configuration.

```bash
dvc init
git add .
```

2. Add the dataset to be versioned from DVC.

```bash
dvc add data/ESC-50
git add data/ESC-50.dvc
```

3. Try removing the dataset and fetching it from the local cache:

```bash
rm -r data/ESC-50
dvc status
dvc checkout
```

4. Add a remote storage for your files. For this part, we simply use a separate folder from the computer. See [the DVC guide](https://dvc.org/doc/command-reference/remote/add) for a full list of possible remote repositories.

```bash
dvc remote add -d localremote <path>
git add .dvc/config
dvc push
```

5. Commit everything, then try to recover the dataset from a fresh clone of the repository:

```bash
git commit -m "Added DVC support"
git push
cd <some-other-folder>
git clone <path-to-repository>
dvc pull
```

## Exercise 3.2 - Setup a more realistic remote

Having a "local remote" DVC storage is, of course, limited. We now investigate setting up a more realistic remote. You can use a number of [possible alternatives](https://dvc.org/doc/command-reference/remote/add). Here, we simulate a Google-Drive Storage

1. Configure the storage from here: (https://dvc.org/doc/user-guide/setup-google-drive-remote#url-format)

5. Try again point 5 from Exercise 3.1.

## Exercise 3.3 - Download a folder from a DVC repository

You can use DVC to download files and folders from a DVC repository. For example, from outside this repository, try:

```bash
dvc list <path-to-repo> data
```

Download the data folder using the default remote:

```bash
dvc get <path-to-repo> data/ESC-50
```

You can also list and/or get from a GitHub repository. For example, from this repository:

```bash
dvc list https://github.com/sscardapane/reprodl2021.git --rev exercise3_dvc_completed data
```

