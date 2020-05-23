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
  You can build the image locally using s2i, by executing the following command
  in the root of your repository. <br>
    `s2i build . quay.io/thoth-station/s2i-custom-notebook:latest MY-CUSTOM-IMAGE:latest`

You might need to update your deployment of JupyterHub for the newly built/imported image to
be available for use.

# Running the image Locally
After building the image, you can run it locally using the following command: <br>
`podman run -p 8080:8080 MY-CUSTOM-IMAGE:latest start-singleuser.sh --ip="0.0.0.0" --port=8080`
