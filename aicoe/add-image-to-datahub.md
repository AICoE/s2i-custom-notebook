# Import Container Image to Internal Datahub:

1. Create a new ImageStream template, you can use [this template](https://github.com/AICoE/s2i-custom-notebook#how-to-add-the-image-to-open-data-hubjupyterhub).
2. Fork this repo https://gitlab.cee.redhat.com/data-hub/dh-jupyterhub.
3. Add a new ImageStream to the [bases/custom-images/jupyterhub-custom-images.yaml](https://gitlab.cee.redhat.com/data-hub/dh-jupyterhub/blob/master/bases/custom-images/jupyterhub-custom-images.yaml).
4. And create a Merge Request to publish these changes to the internal DataHub instance.

The newly created Merge Request will be then reviewed and tested by the DataHub team, and when after all the testing the merge request is merged, your notebook image should be available in the internal Datahub/Jupyterhub instance.

You can look at one of our previous [Merge Requests](https://gitlab.cee.redhat.com/data-hub/dh-jupyterhub/merge_requests/27) to see what the process looks like.
