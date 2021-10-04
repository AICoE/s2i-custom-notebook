s2i-custom-notebook
===================

Image Template to build custom notebook images for JupyterHub.

.. image:: https://img.shields.io/github/v/tag/AICoE/s2i-custom-notebook?style=plastic
  :target: https://github.com/AICoE/s2i-custom-notebook/releases
  :alt: GitHub tag (latest by date)

List of available custom images
-------------------------------

Python 3.6 based custom image `s2i-custom-notebook <https://quay.io/repository/thoth-station/s2i-custom-notebook?tab=tags>`_

.. image:: https://quay.io/repository/thoth-station/s2i-custom-notebook/status
  :target: https://quay.io/repository/thoth-station/s2i-custom-notebook?tab=tags
  :alt: Quay - Build

Python 3.8 based custom image `s2i-custom-py38-notebook <https://quay.io/repository/thoth-station/s2i-custom-py38-notebook?tab=tags>`_

.. image:: https://quay.io/repository/thoth-station/s2i-custom-py38-notebook/status
  :target: https://quay.io/repository/thoth-station/s2i-custom-py38-notebook?tab=tags
  :alt: Quay - Build

Elyra based custom image `s2i-elyra-custom-notebook <https://quay.io/repository/thoth-station/s2i-elyra-custom-notebook?tab=tags>`_

.. image:: https://quay.io/repository/thoth-station/s2i-elyra-custom-notebook/status
  :target: https://quay.io/repository/thoth-station/s2i-elyra-custom-notebook?tab=tags
  :alt: Quay - Build

Python 3.8 based custom image for Red Hat internal use: s2i-custom-py38-internal-notebook
``docker-registry.upshift.redhat.com/aicoe-notebooks/s2i-custom-py38-internal-notebook:stable``

Usage
=====

Set up project repository
-------------------------

1. In your project repository, create a directory named ``notebooks``.
   (The contents of this directory will be copied to the resulting
   image.)
2. Add your notebooks and any other required data to the ``notebooks``
   directory.
3. Add the required Python dependencies for the notebooks in a
   ``Pipfile`` and also lock the dependencies in a ``Pipfile.lock``.
   (All dependencies will be installed during image build time)
4. Choose the suitable base image based on the project requirement from the `List of available custom images`_.
5. Build the image.

How to build the image
----------------------

Locally
~~~~~~~

Prerequisites
^^^^^^^^^^^^^

-  **s2i**: We assume the majority of users will be using linux and
   recommend using the the instructions
   `here <https://github.com/openshift/source-to-image#for-linux>`__ to
   install s2i if needed.

-  **Docker**: If you are using Fedora please use these
   `instructions <https://docs.docker.com/engine/install/fedora/>`__ to
   install docker. If you are on RHEL8 you can run into issues with
   docker, please follow the additional documentation that can be found
   `here <docs/RHEL8_docker_install.md>`__.

Build
^^^^^

You can build the image locally using s2i, by executing the following
command in the root of your repository.

::

    $ s2i build . quay.io/thoth-station/s2i-custom-notebook:latest MY-CUSTOM-IMAGE:latest

You can also build the image directly using your repository url.

::

    s2i build my.repo.url.git quay.io/thoth-station/s2i-custom-notebook:latest MY-CUSTOM-IMAGE:latest

Be sure to replace ``MY-CUSTOM-IMAGE`` above with the name you intend to
use for your image.

You might need to update your deployment of JupyterHub for the newly
built/imported image to be available for use.

Running the image Locally
=========================

After building the image, you can run it locally using the following
command:

::

    $ docker run -p 8080:8080 MY-CUSTOM-IMAGE:latest start-singleuser.sh --ip="0.0.0.0" --port=8080

If you prefer to use `podman <https://podman.io/>`__, you can using the
following command:

::

    $ podman run -p 8080:8080 docker-daemon:MY-CUSTOM-IMAGE:latest start-singleuser.sh --ip="0.0.0.0" --port=8080

Since the s2i notebook builder relies on docker, if using podman, make
sure to prepend your image name with ``docker-daemon:`` so that podman
checks the correct local registry.

How to add the image to Open Data Hub/JupyterHub
------------------------------------------------

-  Once you have pushed your image to an image registry that is
   accessible by your JupyterHub instance, you can use the
   ``ImageStream`` template in this repository at
   ``openshift/oc-imagestream.yaml``.

.. code:: yaml

    ---
    apiVersion: image.openshift.io/v1
    kind: ImageStream
    metadata:
      labels:
        opendatahub.io/notebook-image: "true" # <-- setting it true makes it visible to ODH/JupyterHub
        opendatahub.io/notebook-image-name: "My Notebook Image with Python 3.6"
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

-  After updating the template file with your image details, you can run
   the following command to create a ImageStream in your JupyterHub
   Namespace:

``oc apply -f openshift/oc-imagestream.yaml -n MY_JUPYTERHuB_NAMESPACE``

*In OpenDataHub v0.5 or lower, the ImageStream name should have the
following format: ``s2i-XXXX-notebook``*

-  Once the ImageStream has been created, you might need to restart the
   JupyterHub pod for it see the newly added ImageStream.

How to build the custom base image
----------------------------------

Instead of using the pre-built version of the custom notebook, you can
build the custom notebook from source code. we follow **overlay** based
method in s2i-custom-notebook build. A tool Thamos is used for the
installation of python stacks. Details about the tool can be found at
`Thamos
Documentation <https://github.com/thoth-station/thamos#support-for-multiple-runtime-environments>`__

Example for building python3.6 based custom base image:

-  Build python36 from the **overlay/python36**

`` podman build -t s2i-custom-notebook -f overlays/python36/Dockerfile .``

-  Build python38 from the **overlay/python38**

`` podman build -t s2i-custom-py38-notebook -f overlays/python38/Dockerfile .``

-  Build python38 from the **overlay/python38-internal**

`` podman build -t s2i-custom-py38-internal-notebook -f overlays/python38-internal/Dockerfile .``

-  Build elyra-custom from the **overlay/elyra**

`` podman build -t s2i-elyra-custom-notebook -f overlays/elyra/Dockerfile .``
