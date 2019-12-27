FROM quay.io/openshift/origin-jenkins-agent-base:latest

MAINTAINER Tomas Liumparas <tomas.liumparas@gmail.com>

ENV PATH=$HOME/.local/bin/:$PATH \
    PYTHON_VERSION=3.6 \
    PYTHONUNBUFFERED=1 \
    PYTHONIOENCODING=UTF-8 \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    PIP_NO_CACHE_DIR=off

ENV SUMMARY="Jenkins slave running $PYTHON_VERSION" \
    DESCRIPTION="Jenkins slave running $PYTHON_VERSION is used for running Jenkins python builds in containers"

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Jenkins slave Python 3.6" \
      io.openshift.tags="jenkins-slave,python,python36,python-36,rh-python36" \
      name="centos/jenkins-slave-python-36" \
      version="1" \
      usage="s2i build https://github.com/sclorg/s2i-python-container.git --context-dir=3.6/test/setup-test-app/ centos/python-36-centos7 python-sample-app" \
      maintainer="Tomas Liumparas <tomas.liumparas@gmail.com>"

# Install NodeJS
RUN curl https://raw.githubusercontent.com/cloudrouter/centos-repo/master/CentOS-Base.repo -o /etc/yum.repos.d/CentOS-Base.repo && \
    curl http://mirror.centos.org/centos-7/7/os/x86_64/RPM-GPG-KEY-CentOS-7 -o /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 && \
    DISABLES="--disablerepo=rhel-server-extras --disablerepo=rhel-server --disablerepo=rhel-fast-datapath --disablerepo=rhel-fast-datapath-beta --disablerepo=rhel-server-optional --disablerepo=rhel-server-ose --disablerepo=rhel-server-rhscl" && \
    yum $DISABLES install -y --setopt=tsflags=nodocs epel-release centos-release-scl && \
    yum $DISABLES install -y --setopt=tsflags=nodocs rh-python36 rh-python36-python-devel rh-python36-python-setuptools rh-python36-python-pip && \
    yum clean all -y

RUN source scl_source enable rh-python36 && \
    chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001
