# s2i-custom-notebook
Image Template to build custom notebook images for JupyterHub.

# Usage
1. Fork this repository.
2. Add your notebooks and any other required data to the `notebooks` directory.
3. Add the required Python dependencies in the `Pipfile` and also update the `Pipfile.lock`.
4. Build the image.

## How to build the image

### Locally
  You can build the image locally using s2i, by executing the following command
  in the root of your fork. <br>
    `s2i build . quay.io/thoth-station/s2i-minimal-notebook:latest MY-CUSTOM-IMAGE:latest`

### On OpenShift
  To build this image on OpenShift, we have provided a BuildConfig template in the `openshift`
  directory.

You might need to update your deployment of JupyterHub for the newly built/imported image to
be available for use.

# Running the image Locally
After building the image, you can run it locally using the following command: <br>
`podman run -p 8080:8080 MY-CUSTOM-IMAGE:latest start-singleuser.sh --ip="0.0.0.0" --port=8080`
