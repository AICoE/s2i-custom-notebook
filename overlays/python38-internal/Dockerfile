# s2i-custom-py38-notebook
FROM quay.io/thoth-station/s2i-minimal-py38-notebook:v0.1.1
MAINTAINER Anand Sanmukhani <asanmukh@redhat.com>
LABEL io.k8s.description="Custom JupyterHub Notebook Builder Image" \
      io.openshift.s2i.scripts-url=image:///opt/app-root/builder \
      io.s2i.scripts-url=image:///opt/app-root/builder

ENV \
    # DEPRECATED: Use above LABEL instead, because this will be removed in future versions.
    STI_SCRIPTS_URL=image:///opt/app-root/builder \
    # Path to be used in other layers to place s2i scripts into
    STI_SCRIPTS_PATH=/opt/app-root/builder

USER root

# Trust the Red Hat IT Root CA 
RUN curl -o /etc/pki/ca-trust/source/anchors/RH-IT-Root-CA.crt --fail -L \
         https://password.corp.redhat.com/RH-IT-Root-CA.crt \
 && update-ca-trust extract
 
RUN dnf install -y \
    # runtime dependencies
    krb5-workstation \
    # development dependencies
    krb5-devel \
  # clean up
  && dnf clean all

# Install the Kerberos configuration
COPY ./s2i/config/krb5.conf /etc

USER default

# rename thoth assemble and run scripts
RUN mv /opt/app-root/builder/assemble /opt/app-root/builder/assemble.original
RUN mv /opt/app-root/builder/run /opt/app-root/builder/run.original

# Copy the builder files into /opt/app-root
COPY ./s2i/bin/* /opt/app-root/builder/

# Copy sample notebook to the image
COPY --chown=1001:root ./notebooks /opt/app-root/backup
