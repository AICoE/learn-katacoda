#### Part 3: Installing notebook dependencies

<p align="justify">

Click the `Install` button.
<br>
What happens now is that the [Thoth] resolution engine will resolve and lock down the dependencies such that the versions are optimal, i.e. based on the information stored in the knowledge base, it decides which versions are most compatible together, yield the highest performance w.r.t a given metric (esp. for ML frameworks), have no CVEs<sup>*</sup>.
<br>
Open up the `Web developer tools` (in most browsers press the `F12` key). In the console, you should see logs from the resolution process.
</p>

<p align="justify">

<blockquote>
<i class="fas fa-exclamation"></i> Again, we can check that the dependencies have been <b>locked</b> and saved successfully by examining `requirements_locked`:
<br>

```
oc exec \
    $( oc get pod -l app=jupyter-notebook -o name | sed -e 's/pod\///' ) \
    cat katacoda-examples/jupyter-notebooks/example.ipynb        \
| jq -c '.metadata.requirements_locked'
```{{execute}}

</blockquote

</p>

<p align="justify">

Try running the notebook now. It turns out there is a missing dependency: `ipywidgets`. In this particular case, that is because the package is in fact **not being used** and is therefore discarded from the dependencies as we track only library *usage*. You can click the `+` button and add this package manually. Then, repeat the process of installation.

</p>

<br>

---

<p align="justify">
<sup>*</sup>CVE stands for *Common Vulnerabilities and Exposures*. It is a publicly known security issue or vulnerability of a particular package or a piece of software.
</p>

[Thoth]: https://thoth-station.ninja/
