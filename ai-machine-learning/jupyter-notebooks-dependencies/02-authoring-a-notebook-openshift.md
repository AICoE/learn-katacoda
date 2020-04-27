<p align="justify">

Let's first run the Jupyter Notebook server using our [s2i-minimal-notebook](https://github.com/thoth-station/s2i-minimal-notebook) image stored on [Quay.io](https://quay.io/).

</p>

To import the image to OpenShift, issue:

``oc import-image quay.io/thoth-station/s2i-minimal-notebook s2i-minimal-notebook --confirm``{{execute}}

<p align="justify">

This image contains built-in extensions which may come in useful for this scenario, in particular `Jupyter NBRequirements`.

</p>

<p align="justify">

Now, as learnt in the prerequisite courses, we will deploy that notebook image using a slightly modified `notebook-deployer` template which is for convenience already stored in the root folder.

<br>
To create that template:

``oc create -f notebook-deployer.yaml``{{execute}}

</p>

And to finally serve the notebook:

```
oc process notebook-deployer \
    -p NOTEBOOK_PASSWORD="developer" \
    -p PRELOAD_REPOS="https://github.com/CermakM/katacoda-examples" \
    | oc apply -f -
```{{execute}}

<p align="justify">

Notice the `PRELOAD_REPOS` parameter, which allows us to populate our repository on notebook server startup with a custom Git Hub repository.
<br>
Now you can switch to the OpenShift console to see whether the notebook has been deployed correctly.
<br>

For the credentials, enter:

</p>

* **Username:** ``developer``{{copy}}
* **Password:** ``developer``{{copy}}

<p align="justify">

Go to the project `myproject` and under `Workloads` tab, there should be an application with the Jupyter Notebook server running called `notebook`.

</p>