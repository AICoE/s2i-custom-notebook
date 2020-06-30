# Import Container Image to Internal Datahub:

- Create a new ImageStream template, you can use [this template](https://github.com/AICoE/s2i-custom-notebook#how-to-add-the-image-to-open-data-hubjupyterhub).
- Fork this [repository](https://github.com/AICoE/aicoe-sre).
- Add a new ImageStream to the [repository](https://github.com/AICoE/aicoe-sre/blob/master/applications/jupyterhub/bases/custom-images/jupyterhub-custom-images.yaml).
- Create a Pull Request to publish these changes to the internal DataHub instance.

The newly created Pull Request will be then reviewed and tested by the DataHub team, and when after all the testing the merge request is merged, your notebook image should be available in the internal Datahub/Jupyterhub instance.

For further reference, have a look at one of our previous [Pull Requests](https://github.com/AICoE/aicoe-sre/pull/25) to see what the process looks like.
