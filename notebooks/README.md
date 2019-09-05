# Jupyter Notebooks repo for variation services tutorial

## Create an environment and install requirements:

```bash
source dev_env.sh
```

## Execute non-interactively

To execute a NB non-interactively, use `jupyter nbconvert`. For example:

```bash
jupyter nbconvert --to notebook --execute mynb.ipynb --output=mynboutput.ipynb
```

or

```bash
jupyter nbconvert --to=html --execute mynb.ipynb
```

## Use the notebook interactively

It is strongly recommended to setup Jupyter notebook server with password protection.

### 1. Create a configuration file:

```bash
jupyter notebook --generate-config
```

This will create the configuration file at:

```bash
~/.jupyter/jupyter_notebook_config.py
```

### 2. Create a password

```bash
jupyter notebook password

# enter a new password;
# then confirm by enter it again
```

### 3. Allow remote access

By default, Jupyter notebook server only allows local access via the `localhost` hostname. To allow remote access, look for the following settings in the config file and set the values as listed below:

```python
c.NotebookApp.allow_origin = '*'

c.NotebookApp.allow_remote_access = True

c.NotebookApp.ip = '0.0.0.0'
```

### 4. Start the server

Start the Jupyter server from the notebook directory
```
jupyter notebook --port 54321
```

Then from a web browser (e.g. Chrome) on your PC, visit the following URL with the same port used by the server (assuming the Jupyter notebook server was started on `iebdev12`):

```
http://iebdev12:54321/tree
```

It will show the password page. Enter the password you just created, and the homepage should load, where you can create new notebooks or open existing ones, and edit/execute them.

### 5. Sanitize the notebook

Before committing your notebook, run `sanitize_notebooks` to normalize it so the diffs are easier to understand in pull-requests:

```bash
python3 sanitize_notebooks.py .
```

### 6. Alternative ways to start Jupyter server

You can also run Jupyter notebook server with X-windows server (e.g from a X-Win32 terminal):

```bash
jupyter notebook &
```

This will automatically launch a `konqueror` browser. However, as of this writing, the browser shows a blank page. Alternatively, you can launch a Firefox browser from the same host (e.g. from a X-win32 terminal) which should load the site properly.


## Using GitHub Jupyter notebook with Google Colab

Goolge Colab can open a notebook from GitHub directly.

1. Go to https://colab.research.google.com/github/
2. Enter the github URL of the notebook, and hit enter. Example URL: https://github.com/ncbi/dbsnp/blob/master/tutorials/Variation%20Services/Jupyter_Notebook/navs_spdi_demo.ipynb
3. And then click on the result row to open the notebook in Google Colab

### Upload and install custom packages in Colab

When you are making changes to your Python package used by notebook (e.g. during test), you can upload the updated package directly to Colab (otherwise you need to upload it to public PyPI first).

While the notebook is opened in Colab, copy-paste the following code into the notebook (a new code block):

```python
from google.colab import files
files.upload()
```

When that code is executed: a "Choose Files" button will appear which allows you to browse and upload local file. Choose the package and upload it. E.g. the package file is `ncbi_cloudblast_api-0.0.tar.gz`, then in the notebook, install the package by:

```python
pip install ncbi_cloudblast_api-0.0.tar.gz
```
