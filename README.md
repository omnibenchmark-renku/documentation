# Omnibenchmark documentation

This is the official Omnibenchmark documentation for users; module contributors, benchmarkers and analysts. 

This repo serves: https://omnibenchmark.github.io/documentation/

## How to contribute

1) Install the Python modules in `requirements.txt`.

To install them inside a virtenv you can:

```
## check your path to deactivate conda envs or anything weird there
echo $PATH

# once the path is clean (i.e. with conda deactivate or what needed)
# create a folder to store virtenvs, if not existing, and go there
mkdir -p ~/virtenvs
cd $_

# create a python3 virtualenv with your system's python3
python3 -m venv mkdocs

# activate the virtualenv
source ~/virtenvs/mkdocs/bin/activate

# double check: which pip are we using now?
# should be the one within the `mkdocs` virtenv
which pip 

# go to your home to clone the omni documentation repo there
cd

# clone it (master/main branch)
git clone git@github.com:omnibenchmark/documentation.git

# go to the folder, install using the requirements.txt file
cd documentation
pip install -r requirements.txt
mkdocs --version

# deactivate your virtenv if wanted
deactivate
```

2) Clone the repository and bring your modifications to files in `docs`. If you wish to rearrange or reorder pages, beware to also change `mkdocs.yml`. This file defines how pages are ordered and which files they are based on.

3) When you are done, **test** your modifications locally by running: 

    `mkdocs serve` 

3) If you are happy with the documentation, you can **deploy** them online: 

    `mkdocs gh-deploy`

    The deployment may take some time. You can check the progress on the [deployment history](https://github.com/omnibenchmark/documentation/deployments/activity_log?environment=github  -pages). 

4) commit and push your changes. 


Some useful links: 

- [MkDocs](https://www.mkdocs.org/): basic documentation for MkDocs.

- [MkDocs material](https://squidfunk.github.io/mkdocs-material/): documentation for advanced customization, mainly `themes` and `features` from `mkdocs.yml`. 

- [MkDocs - RealPython](https://realpython.com/python-project-documentation-with-mkdocs/#step-6-host-your-documentation-on-github): explains how to use github for documentation and how to deploy it. 
