FROM gcr.io/cloud-marketplace-tools/testrunner:0.1.5

RUN apt-get update && apt-get install -y --no-install-recommends \
    gnupg2 \
    wget \
    apt-transport-https


RUN wget -q -O /bin/kubectl \
    https://storage.googleapis.com/kubernetes-release/release/v1.24.10/bin/linux/amd64/kubectl \
      && chmod 755 /bin/kubectl

RUN apt-get update && \
    apt-get install -y \
        gnupg \
        wget

RUN wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | apt-key add - && \
    echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-4.4.list


RUN apt-get update -y && \
    apt-get install gnupg && \
    wget -qO- https://www.mongodb.org/static/pgp/server-7.0.asc | tee /etc/apt/trusted.gpg.d/server-7.0.asc && \
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-7.0.list && \
    apt-get install -y mongodb-mongosh && \
    apt-get install -y nano && \
    apt-get install -y jq && \
    apt-get install -y curl


COPY tests/mongo.yaml /tests/mongo.yaml
COPY tests/buyer-app-services.yaml /tests/buyer-app-services.yaml
COPY tester.sh /tester.sh

WORKDIR /

RUN chmod +x tester.sh

ENTRYPOINT ["/tester.sh"]