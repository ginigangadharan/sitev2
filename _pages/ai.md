---
layout: postpage
title: Artificial Intelligence (AI) - Learning and Development
categories: [ AI, Gen AI, ML ]
image: "assets/images/2024/igor-omilaev-eGGFZ5X2LnA-unsplash-ai.jpg"
tags: [ ai, ml, mlops, chatgpt, instructlab, openshift, podman ai]
permalink: ai
featured: false
hidden: false
titleshort: Artificial Intelligence
---

![AI, ML, DL and GenAI](assets/images/2024/ai-ml-dl-genai.png)
*Image source: [YouTube](https://www.youtube.com/watch?v=tZj8j3fdXy4)*

## Introduction - AI, ML, DL, and Gen AI

- Artificial Intelligence (AI): The broader field of creating intelligent agents, aiming to mimic human intelligence.
- Machine Learning (ML): A subset of AI focused on teaching computers to learn from data without explicit programming.
- Deep Learning (DL): A subset of ML using artificial neural networks to analyze complex patterns in data.
- Generative AI (Gen AI): A subfield focused on creating new content, such as text, images, or code, based on learned patterns.

**[InstructLab](https://www.redhat.com/en/topics/ai/what-is-instructlab)** is a game-changer for enhancing large language models (LLMs). This open-source project, co-created by IBM and Red Hat, makes it easier to align LLMs with user intent, opening up possibilities for innovative AI applications.

**[Podman AI Lab](https://podman-desktop.io/docs/ai-lab)** is Red Hat's answer to simplifying the AI development process. This extension provides a local environment with essential open-source tools and curated recipes to guide you through building AI solutions.

**[Red Hat OpenShift AI](https://www.redhat.com/en/technologies/cloud-computing/openshift/openshift-ai)** is a powerful platform for deploying and scaling AI applications across hybrid cloud environments. Built on open-source technologies, it offers a trusted foundation for teams to experiment, serve models, and deliver innovative AI-driven apps.

**Table of Contents**

- [Introduction - AI, ML, DL, and Gen AI](#introduction---ai-ml-dl-and-gen-ai)
- [Getting started with InstructLab](#getting-started-with-instructlab)
  - [Prerequisites](#prerequisites)
  - [Installing InstructLab](#installing-instructlab)
  - [Initialize `ilab`](#initialize-ilab)
  - [Download a Model](#download-a-model)
  - [Serving model](#serving-model)
- [Glossary](#glossary)
- [Some facts](#some-facts)
- [References](#references)

## Getting started with InstructLab

### Prerequisites

**System Requirements:**

- Operating System: Currently supported on Apple M1/M2/M3 Macs and Linux systems.
- Software:
  - C++ compiler
  - Python 3.9 or later
- Storage: Approximately 60GB of free disk space. Note that this requirement can vary based on the size of the LLM you're working with. Please be aware that the actual disk space needed will depend on several factors, including the size of the LLM model itself.

*NOTE: The following commands are executed from a Fedora 40 machine (32GB/12vCPU).*

### Installing InstructLab

- Create a Python virtual environment.

*This is an optional step to isolate the testing environment. You can deploy the `ilab` and components directly into your Python environment if required.*

```shell
$ python3 -m venv aienv
$ cd aienv
$ source bin/activate
(aienv) $ aienv
```

- Install `ilab` CLI

This task  will take some time and also consume some disk space (~5GB) as there will be a lot of dependencies to install.

```shell
$ pip3 install git+https://github.com/instructlab/instructlab.git@v0.16.1

# Check version
$ ilab --version
ilab, version 0.16.1

# Sub-commands
Commands:
  chat      Run a chat using the modified model
  check     (Deprecated) Check that taxonomy is valid
  convert   Converts model to GGUF
  diff      Lists taxonomy files that have changed since...
  download  Download the model(s) to train
  generate  Generates synthetic data to enhance your example data
  init      Initializes environment for InstructLab
  list      (Deprecated) Lists taxonomy files that have changed since...
  serve     Start a local server
  sysinfo   Print system information
  test      Runs basic test to ensure model correctness
  train     Takes synthetic data generated locally with `ilab...
```

### Initialize `ilab`

Prepare the workspace (any directory) and init ilab.

```shell
$ ilab init
Welcome to InstructLab CLI. This guide will help you to setup your environment.
Please provide the following values to initiate the environment [press Enter for defaults]:
Path to taxonomy repo [taxonomy]:
`taxonomy` seems to not exist or is empty. Should I clone https://github.com/instructlab/taxonomy.git for you? [y/N]: y
Cloning https://github.com/instructlab/taxonomy.git...
Generating `config.yaml` in the current directory...
Initialization completed successfully, you're ready to start using `ilab`. Enjoy!
```

### Download a Model

```shell
$ ilab download --repository instructlab/granite-7b-lab-GGUF --filename=granite-7b-lab-Q4_K_M.gguf

$ ls models/
granite-7b-lab-Q4_K_M.gguf
```

### Serving model

With the model downloaded, we're ready to serve it. Serving means setting up a system that allows other programs to interact with the model. It's like creating an API where other applications can send requests and receive responses.

```shell
$ ilab serve --model-path models/granite-7b-lab-Q4_K_M.gguf
INFO 2024-07-26 13:12:46,897 lab.py:340 Using model 'models/granite-7b-lab-Q4_K_M.gguf' with -1 gpu-layers and 4096 max context size.
INFO 2024-07-26 13:12:48,483 server.py:206 Starting server process, press CTRL+C to shutdown server...
INFO 2024-07-26 13:12:48,483 server.py:207 After application startup complete see http://127.0.0.1:8000/docs for API.
```

Now the model is serving and waiting for any request.

You can test the model by starting `ilab model chat` (or `ilab chat`) in another terminal as follows.

Activate Python venv

```shell
$ cd ~/aienv/
$ source bin/activate
(aienv)
```

Goto the InstructLab directory (or use absoluate path)

```shell
$ cd ~/workarea/ilab/

$ ilab chat -m models/granite-7b-lab-Q4_K_M.gguf
╭──────────────────────────────────────────────── system ────────────────────────────────────────────────╮
│ Welcome to InstructLab Chat w/ MODELS/GRANITE-7B-LAB-Q4_K_M.GGUF (type /h for help)                    │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────╯
>>>                                                                                           [S][default]
```

Now we can ask questions here.

```shell
>>> what is a container in 20 words?                                                          [S][default]
╭────────────────────────────────── models/granite-7b-lab-Q4_K_M.gguf ───────────────────────────────────╮
│ A container is a lightweight, portable, and executable software package containing code, libraries,    │
│ dependencies, and system tools. It offers consistent, efficient, scalable, and isolated application    │
│ deployment across various environments.                                                                │
╰─────────────────────────────────────────────────────────────────────────────── elapsed 13.398 seconds ─╯
>>>                                                                                           [S][default]
```

Exit from the chat prompt.

```shell
>>> exit
```


## Glossary

- LAB - Large-scale Alignment for chatBots
- LLM - Large Language Models
- Gen AI - generative artificial intelligence

## Some facts

- Llama2–70B model — a large language model released by [Meta.ai](https://www.meta.ai/).

## References

- [InstructLab Project](https://github.com/instructlab)
- [InstructLab Taxonomy Repo](https://github.com/instructlab/taxonomy)
- [InstructLab Repo](https://github.com/instructlab/instructlab)
- [JJ and Paul continue introducing and getting started with InstructLab (2024)](https://www.youtube.com/watch?v=UArqlnAxNWk) (Video)
- [InstructLab – “Ever imagined the ease of tuning pre-trained LLMs? InstructLab makes it a reality. Let’s delve into how it sets itself apart from other model tuning methods.”](https://medium.com/@syeda9118/instructlab-ever-imagined-the-ease-of-tuning-pre-trained-llms-3331ccea8d88) (Blog)
- [Generative AI Development with Podman AI Lab, InstructLab, & OpenShift AI](https://www.youtube.com/watch?v=tZj8j3fdXy4) (Video)