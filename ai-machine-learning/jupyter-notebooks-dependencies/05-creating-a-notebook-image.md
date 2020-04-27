Suppose we want to do one of the following:
- turn the notebook into an executable so that it could be used as an entrypoint to (for example) a `CronJob`
- deploy the notebook regularly, for example to perform a data analysis (just with different inputs)
- share the notebook with others so that they could just spawn it locally and experiment with it

<p align="justify">

For all those reasons above, it might be useful to be able to turn even a single notebook into an image and push it to an image registry, such as [quay.io](https://quay.io/search) or [Docker Hub](https://hub.docker.com/).

</p>

<p align="justify">

At this point, this is extremely simple to do and it is just a cherry on the cake. We can, in fact, use the s2i locally in combination with a special Jupyter Notebook builder image.

</p>


**Option A)**

Using OpenShift s2i:

``oc create -f notebook-builder.json``{{execute}}

```
oc process notebook-builder \
    -p BUILDER_IMAGE="quay.io/cermakm/jupyter-notebook-s2i:latest"       \
    -p GIT_REPOSITORY_URL="https://github.com/CermakM/katacoda-examples" \
    -p CONTEXT_DIR="jupyter-notebooks"                                   \
    -p NOTEBOOK_PATH="example-with-requirements.ipynb"                   \
    | oc apply -f -
```{{execute}}

This command creates a build which you can list by:

``oc get builds``{{execute}}

Once the build is done, we can create a simple `Job` to execute the notebook:

```
cat <<EOF | oc apply -f -
apiVersion: batch/v1
kind: Job
metadata:
  name: jupyter-notebook
spec:
  template:
    spec:
      containers:
      - name: notebook
        image: jupyter-notebook:latest
        # use command if you want to run the notebook server
        # in this case, you'll have to create routes and services
        # similarly to the previous scenario
        # command: "/opt/app-root/bin/jupyter"
      restartPolicy: Never
  backoffLimit: 4
EOF
```{{execute}}

**Option B)**

Using [s2i](https://github.com/openshift/source-to-image) binary locally

``IMAGE_NAME="jupyter-notebook"``{{execute}}

```
curl -sSL https://github.com/openshift/source-to-image/releases/download/v1.2.0/source-to-image-v1.2.0-2a579ecd-linux-amd64.tar.gz | tar -C /usr/bin -xzf -

s2i build \
    -c katacoda-examples/jupyter-notebooks/     \
    --env JUPYTER_NOTEBOOK_PATH='example-with-requirements.ipynb' \
    quay.io/cermakm/jupyter-notebook-s2i:latest ${IMAGE_NAME}
```{{copy}}

> NOTE: `docker` is not available in this environment, you may try these locally or in a dedicated Katacoda docker environment.

<p align="justify">

You now have an image called `${IMAGE_NAME}:latest` which is able to execute the notebook **as a script**. The only thing we need to provide is the `JUPYTER_NOTEBOOK_PATH` env variable so that the entrypoint knew where to look for the notebook.

</p>

<p align="justify">

Again, as docker is not available in this environment, we can't run these directly here in the terminal. The following command would execute the notebook as a script:

</p>

```
CONTAINER_NAME="jupyter-notebook"
docker run -it \
    --env JUPYTER_NOTEBOOK_PATH='example-with-requirements.ipynb' \
    --name "$CONTAINER_NAME"                    \
    ${IMAGE_NAME}:latest
```{{copy}}

Whereas this would spawn the notebook server:

```
CONTAINER_NAME="jupyter-notebook"
docker run -it \
    --entrypoint "/opt/app-root/bin/jupyter" \
    --name "$CONTAINER_NAME"                 \
    ${IMAGE_NAME}:latest
```{{copy}}