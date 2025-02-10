#ddev-generated
ARG OPENSEARCH_TAG=latest

FROM busybox:latest as build-stage

RUN wget -O sudachi.zip https://github.com/WorksApplications/elasticsearch-sudachi/releases/download/v3.3.0/opensearch-2.18.0-analysis-sudachi-3.3.0.zip
RUN wget -O dict-core.zip https://github.com/WorksApplications/SudachiDict/releases/download/v20241021/sudachi-dictionary-20241021-core.zip
RUN unzip dict-core.zip


FROM opensearchproject/opensearch:${OPENSEARCH_TAG}

ARG INSTALL_PLUGIN_ANALYSIS_PHONETIC=true
ARG INSTALL_PLUGIN_ANALYSIS_ICU=true

WORKDIR /usr/share/opensearch

# Sudach installation
COPY --from=build-stage sudachi.zip .
COPY --from=build-stage sudachi-dictionary-20241021/system_core.dic config/sudachi/system_core.dic
RUN /usr/share/opensearch/bin/opensearch-plugin install --batch file:sudachi.zip
RUN /usr/share/opensearch/bin/opensearch-plugin install --batch repository-s3

# Install plugins
RUN if [ "${INSTALL_PLUGIN_ANALYSIS_PHONETIC}" = "true" ]; then bin/opensearch-plugin install analysis-phonetic; fi
RUN if [ "${INSTALL_PLUGIN_ANALYSIS_ICU}" = "true" ]; then bin/opensearch-plugin install analysis-icu; fi
