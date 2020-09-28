# s2i-custom-notebook
Image Template to build custom notebook images for JupyterHub.

# Usage
## Set up project repository
1. In your project repository, create a directory named `notebooks`. (The contents of this directory will be copied to the resulting image.)
2. Add your notebooks and any other required data to the `notebooks` directory.
3. Add the required Python dependencies for the notebooks in a `Pipfile` and also lock the dependencies in a `Pipfile.lock`. (All dependencies will be installed during image build time)
4. Build the image.

## How to build the image

### Locally

#### Prerequisites

* **s2i**: We assume the majority of users will be using linux and recommend using the the instructions
[here](https://github.com/openshift/source-to-image#for-linux) to install s2i if needed.

* **Docker**: If you are using Fedora please use these [instructions](https://docs.docker.com/engine/install/fedora/)
to install docker. If you are on RHEL8 you can run into issues with docker, please follow the additional documentation
that can be found [here](docs/RHEL8_docker_install.md).

#### Build

You can build the image locally using s2i, by executing the following command in the root of your repository. <br>
````
$ s2i build . quay.io/thoth-station/s2i-custom-notebook:latest MY-CUSTOM-IMAGE:latest
````
You can also build the image directly using your repository url. <br>
```
s2i build my.repo.url.git quay.io/thoth-station/s2i-custom-notebook:latest MY-CUSTOM-IMAGE:latest
```

Be sure to replace `MY-CUSTOM-IMAGE` above with the name you intend to use for your image.

You might need to update your deployment of JupyterHub for the newly built/imported image to
be available for use.

# Running the image Locally

After building the image, you can run it locally using the following command: <br>
```
$ docker run -p 8080:8080 MY-CUSTOM-IMAGE:latest start-singleuser.sh --ip="0.0.0.0" --port=8080
```
If you prefer to use [podman](https://podman.io/), you can using the following command:
```
$ podman run -p 8080:8080 docker-daemon:MY-CUSTOM-IMAGE:latest start-singleuser.sh --ip="0.0.0.0" --port=8080
```
Since the s2i notebook builder relies on docker, if using podman, make sure to prepend your image name with
`docker-daemon:` so that podman checks the correct local registry.

## How to add the image to Open Data Hub/JupyterHub

* Once you have pushed your image to an image registry that is accessible by your JupyterHub instance,
you can use the `ImageStream` template in this repository at `openshift/oc-imagestream.yaml`.


```yaml
---
apiVersion: image.openshift.io/v1
kind: ImageStream
metadata:
  labels:
    opendatahub.io/notebook-image: "true" # <-- setting it true makes it visible to ODH/JupyterHub
  name: custom-notebook # <-- Update the ImageStream name here
spec:
  lookupPolicy:
    local: true
  tags:
  - from:
      kind: DockerImage
      name: quay.io/thoth-station/s2i-custom-notebook:latest # <-- Change to your container Image with tag
    importPolicy:
      scheduled: true
    name: "latest"
```

* After updating the template file with your image details, you can run the following command to create a ImageStream in your JupyterHub Namespace:

  `oc apply -f openshift/oc-imagestream.yaml -n MY_JUPYTERHuB_NAMESPACE`

  *In OpenDataHub v0.5 or lower, the ImageStream name should have the following format: `s2i-XXXX-notebook`*

* Once the ImageStream has been created, you might need to restart the JupyterHub pod for it see the newly added ImageStream.
