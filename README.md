# About this Repo

This is a repo for preparing *A Hitchhiker’s Guide to Bayesian Hierarchical Drift-Diffusion Modeling with dockerHDDM* 

Please read our preprint at: https://psyarxiv.com/6uzga/

The docker image described by this tutorial can be found at: https://hub.docker.com/r/hcp4715/hddm, with tag `latest`, i.e, 0.9.8.

## Folder structure of the current repo

```
.
│  .dockerignore
│  .gitignore
│  README.md
│              
├─dockerfiles
│  ├─0.8.0
│  │  │  .dockerignore
│  │  │  Dockerfile.amd64
│  │  │  Dockerfile.arm64
│  │  │  README.md
│  │  │  
│  │  └─examples
│  │      
│  └─0.9.8
│          docker-bake.hcl
│          Dockerfile.amd64
│          Dockerfile.arm64
│          README.md
│          README_zh.md
│          
├─example
│          Basic_HDDM_Tutorial.ipynb
│          From_simulator_to_intference_with_HDDM_LAN_version.ipynb
│          LAN_Tutorial.ipynb
│          Posterior_Predictive_Checks.ipynb
│              
│              
└─tutorial
    │  dockerHDDM_primer.ipynb
    │  dockerHDDM_workflow.ipynb
    │  
    └─figs
```

## What is HDDM? 
A python package for hierarchical drift-diffusion models.

### Scope of the current tutorial
Our tutorial is compatible with classic version (0.8.0) and latest (0.9.8) of HDDM. All docker images are available (https://hub.docker.com/r/hcp4715/hddm).

## Three simple steps for using this guide

### Step 1: Install docker on your machine

For Mac users, see [this](https://docs.docker.com/desktop/install/mac-install/) for installation and [this](https://docs.docker.com/desktop/mac/permission-requirements/) for permission requirements. 

For windows users, see [this](https://docs.docker.com/desktop/install/windows-install/) for installation and [this](https://docs.docker.com/desktop/windows/permission-requirements/) for permission requirements.

For Linux users, you may only need the docker engine instead of docker desktop, see the differences [here](https://docs.docker.com/desktop/faqs/linuxfaqs/#what-is-the-difference-between-docker-desktop-for-linux-and-docker-engine). Please see [here](https://docs.docker.com/engine/install/) for installing docker engine in different distributions of Linux.

** Plese verify docker desktop or docker engine is properly installed ** 

For Mac & Windows users, start docker desktop and then run `hello-world` images by the following code in your terminal or command line:

`docker run hello-world`

This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.

For linux users, verification of the installation is part of the instructions, which include code (for Ubuntu) like this:

`sudo docker run hello-world`

### Step 2: Pull the `hddm`

Now that we successfully installed docker and can run docker in terminal or command line, we then pull the docker image for the current tutorial using the code below:

```
docker pull hcp4715/hddm
```

Note that this is also part of our tutorial (see our preprint: https://psyarxiv.com/6uzga/) and code (in `jupyter notebook`, i.e., `./tutorial/dockerHDDM_primer.ipynb` in this repo).

### Step 3: Using HDDM and the tutorial

Now that we successfully pulled the docker image for the tutorial, we can use use the HDDM inside the docker by starting a container. Note that our docker images are compatible with MacOS (M1 chip), Linux and Windows，and you can rest assured to use it.  

run the code below in termial:

```
docker run -it --rm --cpus=5 \
-v $(pwd):/home/jovyan/work \
-p 8888:8888 hcp4715/hddm:latest jupyter notebook
```

`docker run` ---- run a docker image in a container

`-it` ---- Keep STDIN open even if not attached

`--rm` ---- Automatically remove the container when it exits

`--cpus=5` ---- Number of cores will be used by docker, make sure you have more cores than the number here

`-v` ---- mount a folder to the container

`$(pwd)` ---- the current working directory where storing customized script and data. And you could replace it to any path in your computer. 

`/home/jovyan/work` ---- the folder path  where the local folder will be mounted [**do not change this unless you know what you are doing**]

`-p` ---- Publish a container’s port(s) to the host

`hcp4715/ddm:latest` ---- the docker image to run

`jupyter notebook` ---- Open juypter notebook when start running the container.

After running the code above, bash will has output like this:

```
....
To access the notebook, open this file in a browser:
        file:///home/jovyan/.local/share/jupyter/runtime/nbserver-6-open.html
Or copy and paste one of these URLs:
    http://174196acc395:8888/?token=75f1a7a8ffcbb55f0c2802433a9a5d57ac00868e05089c09
 or http://127.0.0.1:8888/?token=75f1a7a8ffcbb55f0c2802433a9a5d57ac00868e05089c09
```

Copy the url (http://127.0.0.1:8888/?.......) to a browser (firefox or chrome) and it will show a web page, this is the interface of jupyter notebook! 

Under the `Files` tab, there should be three folders: `work`, `example`, and `tutorial`. 

- The `example` folder contains several tested example jupyter notebooks from original HDDM. 
- The `tutorial` folder contains two tutorial notebook introduced in [our paper](https://psyarxiv.com/6uzga/), inlcuding [dockerHDDM_primer.ipynb](tutorial/dockerHDDM_primer.ipynb) and [dockerHDDM_workflow.ipynb](tutorial/dockerHDDM_workflow.ipynb), in which you can reproduce the analysis we presented in the tutorial.
- The `work` folder is the local folder mounted in docker container. Enter `work` folder, you can analyze your own data stored in folder of current working directory.


## How this docker image was built

An alternative way to get the docker image is to build it from `Dockerfile`, [more details](dockerfiles/0.9.8/README.md)

I built this docker image under Ubuntu 20.04. 

Code for building the docker image (don't forget the `.` in the end):

```
docker build -t hcp4715/hddm:latest -f Dockerfile .
```
You can replace `hcp4715` with your username in docker hub, and replace `hddm:latest` with a name and tag you prefer.

## Acknowledgement
Thank [@madslupe](https://github.com/madslupe) for his previous HDDM image, which laid the base for the current version. Thank [Dr Rui Yuan](https://scholar.google.com/citations?user=h8_wSLkAAAAJ&hl=en) for his help in creating the Dockerfile.
