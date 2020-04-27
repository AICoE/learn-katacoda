#### Part 1: Opening the notebook

Check out the status of pods:

``oc get pods``{{execute}}

Or wait for the notebook server to be ready directly:

```
oc wait --for=condition=ready pod \
    $( oc get pod -l app=jupyter-notebook -o name | sed -e 's/pod\///' )
```{{execute}}

<p align="justify">

Switch over to the the `Jupyter` tab if everything has been deployed successfully to enter the Jupyter Notebook UI.

<b>NOTE:</b> In some browsers, you might not be able to proceed to the Jupyter login console in the Katacoda tab. You might need to acknowledge the security protocol. In order to do that and open the following URL in a new tab and accept to proceed if the browser asks you to do so. You may proceed further in that browser tab or return to the Katacoda Jupyter tab.

</p>

<a href="https://jupyter-notebook-myproject.[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com">notebook-myproject.[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com</a>
<br>

<center>
<img src="../../assets/jupyternotebooks/jupyter-notebooks-authoring-42/openshift-console.png" alt="Notebook server in OpenShift Console" align="middle">
</center>

<p align="justify">

> **HINT**: If the URL above doesn't work, you can also learn the notebook host address by issuing:

</p>

``echo $(oc get route jupyter-notebook -o jsonpath='{@.spec.host}')``{{execute}}

<p align="justify">

or in the OpenShift Console UI go to `Projects` -> `myproject` -> `Workloads` -> `jupyter-notebook` and click the `Location` URL.

</p>

<center>
<img alt="Login Prompt" src="../../assets/jupyternotebooks/jupyter-workspace-42/04-notebook-login-prompt.png">
</center>

<p align="justify">

At the login prompt, enter the password ``developer``{{copy}} which you entered into the ``NOTEBOOK_PASSWORD`` field. You will then be granted access to the Jupyter notebook file browser.

</p>

<p align="justify">

When in the notebook server, open the notebook stored at ``katacoda-examples/jupyter-notebooks/example.ipynb``. You will notice that the notebook uses some dependencies that **might not be present in the Python environment** (especially assuming that we're running in a clean one).

</p>

These are:
- matplotlib
- mpl_toolkits
- scipy

> <i class="fas fa-exclamation"></i> Try executing the notebook cells. Does it fail?

Let's see how we can deal with that...