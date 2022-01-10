# s2i-elyra-custom-notebook
FROM quay.io/thoth-station/s2i-lab-elyra:v0.1.1
MAINTAINER Anand Sanmukhani <asanmukh@redhat.com>
LABEL io.k8s.description="Custom JupyterHub Notebook Builder Image" \
      io.openshift.s2i.scripts-url=image:///opt/app-root/builder \
      io.s2i.scripts-url=image:///opt/app-root/builder

ENV \
    THAMOS_RUNTIME_ENVIRONMENT="" \
    # DEPRECATED: Use above LABEL instead, because this will be removed in future versions.
    STI_SCRIPTS_URL=image:///opt/app-root/builder \
    # Path to be used in other layers to place s2i scripts into
    STI_SCRIPTS_PATH=/opt/app-root/builder

# rename thoth assemble and run scripts
RUN mv /opt/app-root/builder/assemble /opt/app-root/builder/assemble.original
RUN mv /opt/app-root/builder/run /opt/app-root/builder/run.original

# Copy the builder files into /opt/app-root
COPY ./s2i/bin/* /opt/app-root/builder/

# Copy sample notebook to the image
COPY --chown=1001:root ./notebooks /opt/app-root/backup
