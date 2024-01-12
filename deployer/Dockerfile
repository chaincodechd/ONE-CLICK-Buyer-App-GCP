# ARG MARKETPLACE_TOOLS_TAG
FROM marketplace.gcr.io/google/c2d-debian11 AS build

RUN apt-get update \
    && apt-get install -y --no-install-recommends gettext

ADD chart/ONE-CLICK-Buyer-App /tmp/chart
RUN cd /tmp && tar -czvf /tmp/ONE-CLICK-Buyer-App.tar.gz chart

ADD apptest/deployer/ONE-CLICK-Buyer-App /tmp/test/chart
RUN cd /tmp/test \
    && tar -czvf /tmp/test/ONE-CLICK-Buyer-App.tar.gz chart/

ADD schema.yaml /tmp/schema.yaml

ARG REGISTRY=gcr.io/chaincode-consulting/buyer_app
ARG TAG=1.0.0

RUN cat /tmp/schema.yaml \
    | env -i "REGISTRY=$REGISTRY" "TAG=$TAG" envsubst \
    > /tmp/schema.yaml.new \
    && mv /tmp/schema.yaml.new /tmp/schema.yaml

ADD apptest/deployer/schema.yaml /tmp/apptest/schema.yaml
RUN cat /tmp/apptest/schema.yaml \
    | env -i "REGISTRY=$REGISTRY" "TAG=$TAG" envsubst \
    > /tmp/apptest/schema.yaml.new \
    && mv /tmp/apptest/schema.yaml.new /tmp/apptest/schema.yaml


FROM gcr.io/cloud-marketplace-tools/k8s/deployer_helm:0.11.9
COPY --from=build /tmp/ONE-CLICK-Buyer-App.tar.gz /data/chart/
COPY --from=build /tmp/test/ONE-CLICK-Buyer-App.tar.gz /data-test/chart/
COPY --from=build /tmp/apptest/schema.yaml /data-test/
COPY --from=build /tmp/schema.yaml /data/

ENV WAIT_FOR_READY_TIMEOUT 1800
ENV TESTER_TIMEOUT 1800