# Pypeline

Python DSL for [concourse](https://concourse-ci.org/)


## Installation

Currently, there is no pypi repository avaialable within SAP. Therefore, installation must be done manually

```bash
git clone https://github.tools.sap/cki/pypeline.git
cd pypeline
make install
```

## Example

```python
from pypeline import Pipeline, GitRepo

with Pipeline("c21s", __file__) as pipeline:
    pipeline.resource("shalm", GitRepo("https://github.com/wonderix/shalm"))
    with pipeline.job("create-cluster") as job:
        shalm_dir = job.get("shalm")
        cluster_name = "xxx"


        @job.task()
        def create_shoot():
            print("Create cluster {}".format(cluster_name))
            return cluster_name


        @job.task(secrets={"home": "HOME"})
        def install_shalm(home=None):
            print("HOME=" + home)
            print("Installing shalm {} into {}".format(shalm_dir, create_shoot()))
            return "Hello"


        job = pipeline.job("test-cluster")
        shalm_dir = job.get("shalm")
```
