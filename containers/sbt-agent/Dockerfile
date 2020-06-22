FROM jenkins/slave:latest
LABEL agent=jenkins-slave-sbt

USER root
RUN echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list \
    && curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | apt-key add \
    && apt update \
    && apt install sbt

RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
  && chmod +x ./kubectl && mv ./kubectl /usr/local/bin/kubectl
#RUN curl -LO https://mirror.openshift.com/pub/openshift-v3/clients/3.11.200/linux/oc.tar.gz && tar xzvf oc.tar.gz -C /tmp/ && mv /tmp/oc /usr/local/bin/oc

COPY scripts/jenkins-agent /usr/local/bin/jenkins-agent
RUN chmod +x /usr/local/bin/jenkins-agent &&\
    ln -s /usr/local/bin/jenkins-agent /usr/local/bin/jenkins-slave

COPY repositories /home/jenkins/.sbt/repositories
RUN chown jenkins:jenkins -R /home/jenkins
USER jenkins
ENTRYPOINT ["jenkins-agent"]
