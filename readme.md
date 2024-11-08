# CSCS Knowledge Base Markdown Proof-of-Concept

This is a proof of concept of using MKDocs for the CSCS Knowledge Base.


## Objectives

Ease of deployment:
* easy to make small edits and changes
    * can we have a link to the markdown directly on each page
* easy to preview your changes before deploying them
    * currently the only way to do this is to build the docs locally and view them
    * support for preview pages is [under development](https://github.com/orgs/community/discussions/7730)
* easy to deploy straight away for small changes
* easy to request a review

Search:
* search should just work
* integration of LLM chat bot for answering questions based on docs
* generate links to KB docs on service desk tickets

Front page:
* A custom front page that shows "cards", like the ones on the [Material documentation](https://squidfunk.github.io/mkdocs-material/) or our [Confluence based KB](https://squidfunk.github.io/mkdocs-material/).

* Persistance of links
    * what happens to incoming links if we rename/move a file? (compare with Confluence)

## Workflow

The workflow for updating documentation is.

### small changes

### larger changes

Clone this repository on your PC/laptop, then create a Python venv that provides the tools required to build and review the documentation locally.

```
# create and activate a venv
python3 -m venv pyenv
source pyenv/bin/activate

# install required packages
pip install --upgrade pip
pip install -r requirements.txt
```

You will need to restart the venv for all the tools to work properly.

The following command will build the docs, and start a server for you to view them locally. It will rebuild the docs every time you save one of the markdown/configuration files.

```bash
mkdocs serve
```

This generates output similar to the following, in which you can find the link to view the local docs (http://127.0.0.1:8000/) in this case.

```
mkdocs serve                                                                                                  (pyenv) main [e2006ad] Δ ?
INFO    -  Building documentation...
INFO    -  Cleaning site directory
INFO    -  The following pages exist in the docs directory, but are not included in the "nav" configuration:
             - writing-docs.md
INFO    -  Documentation built in 0.15 seconds
INFO    -  [12:17:14] Watching paths for changes: 'docs', 'mkdocs.yml'
INFO    -  [12:17:14] Serving on http://127.0.0.1:8000/
```

## How it Works
