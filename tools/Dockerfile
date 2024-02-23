FROM debian:bullseye-slim
RUN apt update -y
RUN apt install apt-utils -y
RUN apt install curl openssh-client -y
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash
ENTRYPOINT /bin/bash
