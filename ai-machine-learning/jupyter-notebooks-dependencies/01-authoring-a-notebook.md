<p align="justify">

In order to create an image for a container in which this notebook could run, we need to provide a set of dependencies that the notebook needs.

</p>

<p align="justify">

In a <q>traditional</q> approach, we would create a `requirements.txt` or a `Pipfile` (an eventually `Pipfile.lock`) as so called <q>manifest files</q> and we would publish these along with the notebook to a Git Hub repo. As it was shown in the previous courses, we can then build and deploy a notebook image using that repository.
<br>
However, with this approach, we don't need to do any of that. In fact, we don't even need a Git Hub repository to be able to build an image.

</p>

<p align="justify">

All we need is a `dependency manager`.

</p>