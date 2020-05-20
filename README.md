Dataset Insights
===========
This repo enables users to understand their synthetic datasets by exposing the metrics collected when the dataset
was created e.g. object count, label distribution, etc. The easiest way to use Dataset Insights is
to run the notebook (#todo add path to notebook)
#Requirements
The Dataset Insight notebooks assume that the user has already generated a synthetic dataset using the Unity Perception package. To learn how to create a synthetic dataset using Unity please see the
[perception documentation](https://github.com/Unity-Technologies/com.unity.perception).
(#todo update link)

## Running the Dataset Insights Jupyter Notebook Locally
You can either run the notebook by installing our python package or by using our docker image.

### Running a Notebook Locally Using Docker

#### Requirements
[Docker](https://docs.docker.com/get-docker/) installed.

#### Steps
1. Run notebook server using docker

```bash
docker run \
  -p 8888:8888 \
  -v $HOME/data:/data \
  -t gcr.io/unity-ai-thea-test/thea:latest #todo update docker image url 
```
This command mounts directory `$HOME/data` in your local filesystem to `/data` inside the container. If you are loading a dataset generated locally from a Unity app, replace this path with the root of your app's persistent data folder.

Example persistent data paths from [SynthDet](https://github.com/Unity-Technologies/synthdet):
* OSX: `~/Library/Application Support/UnityTechnologies/SynthDet`
* Linux: `$XDG_CONFIG_HOME/unity3d/UnityTechnologies/SynthDet`
* Windows: `%userprofile%\AppData\LocalLow\UnityTechnologies\SynthDet`


2. Go to `http://localhost:8888` in a web browser to open the Jupyter browser.
3. Open and run the example notebooks in `/datasetinsights/notebooks/` or create your own.

### Running a Notebook Locally Using Our Python Package

#### Requirements

A virtualenv with python3.7

You can setup a virtual environment using [miniconda](https://docs.conda.io/en/latest/miniconda.html) using the following commands:

```bash
conda create -n datasetinsights python=3.7
conda activate datasetinsights
```

You can also use [pyenv](https://github.com/pyenv/pyenv) and
[pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) to create a virtual environment for this project.
This [blog](https://towardsdatascience.com/managing-virtual-environment-with-pyenv-ae6f3fb835f8) is a good tutorial.

Once you have a virtual environment you can install our python package using pip via the following commands:
```bash
python -m pip install --upgrade pip
python -m pip install datasetinsights
```
#### Steps
todo add instructions, IDK where the notebook will actually be

## Running a Dataset Insights Jupyter Notebook via Google Cloud Platform (GCP)
- To run the notebook on GCP's AI platform follow
[these instructions](https://cloud.google.com/ai-platform/notebooks/docs/custom-container) and use the container
todo insert container url, todo verify steps work
- Alternately, to run the notebook on kubeflow follow [these steps](https://www.kubeflow.org/docs/notebooks/setup/)


### Download Dataset from Unity Simulation

[Unity Simulation](https://unity.com/products/simulation) provides a powerful platform for running simulations at large scale. You can use the provided cli script to download Perception datasets generated in Unity Simulation:

```bash
python -m datasetinsights.scripts.usim_download \
  --data-root=$HOME/data \
  --run-execution-id=<run-execution-id> \
  --auth-token=<xxx>
```

The `auth-token` can be genrated using the Unity Simulation [CLI](https://github.com/Unity-Technologies/Unity-Simulation-Docs/blob/master/doc/cli.md#usim-inspect-auth). This script will download the synthetic dataset for the requested [run-execution-id](https://github.com/Unity-Technologies/Unity-Simulation-Docs/blob/master/doc/cli.md#argument-descriptions).

If the `--include-binary` flag is present, the images will also be downloaded. This might take a long time, depending on the size of the generated dataset.
