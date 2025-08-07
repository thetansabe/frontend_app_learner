<a id="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/thetansabe/frontend_app_learner">
    <img src="https://kamimind.ai/icons/favicon.svg" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">Chatbot EdX MFE</h3>

  <p align="center">
    <a href="https://docs.google.com/document/d/18hVomCJ_LPs2bTCooNbRvz-M_LR8pnd6/edit?usp=sharing&ouid=107337327725163773984&rtpof=true&sd=true"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://youtu.be/R-9tuvLTDB4">View Demo</a>
    &middot;
    <a href="https://github.com/thetansabe/chatbot">Visit Chatbot server</a>
    &middot;
    <a href="https://github.com/S1mpleOW/openedx-aws-tf">Visit AWS deployment</a>
  </p>
</div>

<!-- Table of Contents -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->

## About The Project

Installing the open-source edX platform, using a micro-frontend architecture to integrate a chatbot application into the LMS platform. This enhances the developer experience for contributing to the open-source code and adds new AI features that edX currently lacks.

In the demo, you can see that our chatbot app can reuse uri, themes and components from edX Tutor, such as the header bar and authentication page.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Built With

- ![React][React.js]
- ![Python][Python.org]
- ![Open edX][Open edX]
- ![MongoDB][MongoDB]
- ![ChromaDB][ChromaDB]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## Getting Started

Setting up Tutor EdX and the chatbot application requires some prerequisites and installation steps. Follow the instructions below to get started.

### Prerequisites

1. Setup virtual environment (venv) at [miniconda](https://www.anaconda.com/docs/getting-started/miniconda/install#to-download-an-older-version)

2. From now on, you must work in the venv, (recommend Python 3.11.5). Follow syntax at [cheetsheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)

3. Have Docker installed and know how to use it.

### Installation

Download Tutor EdX:

```bash
pip install "tutor[full]"
```

Run Tutor EdX:

```bash
tutor local launch
```

Note:

- This is tutor master branch, the nightly branch have more functions but too many errors and you have to wait too long for fixes
- If launch fail, cmd: “Tutor config printroot”, go to the directory to delete “Data” folder

<p align="right">(<a href="#readme-top">back to top</a>)</p>

#### Run local chatbot app

Make sure you installed both frontend and backend repos.

To run this frontend locally, run:

```bash
npm install
npm start dev
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

#### Run as a micro-frontend inside Tutor EdX

Inspect your Tutor setting, make sure mfe plugin is enabled:

```bash
tutor plugins list
```

You should see the mfe plugin listed. If not, you can enable it by running:

```bash
tutor plugins enable mfe
```

Create plugin folder:

```bash
mkdir -p "$(tutor plugins printroot)"
```

Change directory to the plugin folder:

```bash
cd "$(tutor plugins printroot)"
```

Add a file [yourmfe]plugin.py inside the folder (replace [yourmfe] with your plugin name, in this case, it is "learner"):

Add content to the plugin.py file:

```python
########################################
# CONFIGURATION
########################################
from tutormfe.hooks import MFE_APPS


@MFE_APPS.add()
def _add_my_mfe(mfes):
    mfes["mfe_yourmfe"] = {
        "repository": "https://github.com/thetansabe/frontend_app_[yourmfe].git",
        "port": 2003,
        "version": "master", # optional, will default to the Open edX current tag.
	   "name”:"mfe_yourmfe",
        # "refs" : "https://api.github.com/repos/rimsha-zaib/mfe_courses/git/refs/heads",


    }
    return mfes
```

Save your configs

```bash
tutor plugins enable [yourmfe]plugin
tutor config save
```

Your Dockerfile updated at, example:
C:\Users\AN515-43\AppData\Local\tutor\tutor\env\plugins\mfe\build\mfe

Run your custom mfe:

```bash
tutor images build mfe

tutor local launch
```

After running the above commands, you should be able to access your MFE at:

```bash
http://apps.local.edly.io/mfe_yourmfe/
```

<!-- MARKDOWN LINKS & IMAGES -->

[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[Python.org]: https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=FFFFFF
[Open edX]: https://img.shields.io/badge/Open%20edX-000000?style=for-the-badge&logo=open-edx&logoColor=FFFFFF
[MongoDB]: https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=FFFFFF
[ChromaDB]: https://img.shields.io/badge/ChromaDB-2C3E50?style=for-the-badge&logo=chromadb&logoColor=FFFFFF
