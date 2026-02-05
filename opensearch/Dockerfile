#ddev-generated

# Version reference:
# - OpenSearch Sudachi plugin: https://github.com/WorksApplications/elasticsearch-sudachi/releases
ARG OPENSEARCH_TAG=2.18.0
ARG SUDACHI_PLUGIN_VERSION=3.3.0

# - Sudachi dictionary: https://github.com/WorksApplications/SudachiDict/releases
ARG SUDACHI_DICT_VERSION=20241021


FROM busybox AS unpack
ARG OPENSEARCH_TAG
ARG SUDACHI_PLUGIN_VERSION
ARG SUDACHI_DICT_VERSION
RUN wget https://github.com/WorksApplications/elasticsearch-sudachi/releases/download/v${SUDACHI_PLUGIN_VERSION}/opensearch-${OPENSEARCH_TAG}-analysis-sudachi-${SUDACHI_PLUGIN_VERSION}.zip
RUN wget https://github.com/WorksApplications/SudachiDict/releases/download/v${SUDACHI_DICT_VERSION}/sudachi-dictionary-${SUDACHI_DICT_VERSION}-core.zip
RUN unzip sudachi-dictionary-${SUDACHI_DICT_VERSION}-core.zip


FROM opensearchproject/opensearch:${OPENSEARCH_TAG}
ARG OPENSEARCH_TAG
ARG SUDACHI_PLUGIN_VERSION
ARG SUDACHI_DICT_VERSION

ARG INSTALL_PLUGIN_ANALYSIS_PHONETIC=true
ARG INSTALL_PLUGIN_ANALYSIS_ICU=true

WORKDIR /usr/share/opensearch

# Sudach installation
COPY --from=unpack opensearch-${OPENSEARCH_TAG}-analysis-sudachi-${SUDACHI_PLUGIN_VERSION}.zip .
COPY --from=unpack sudachi-dictionary-${SUDACHI_DICT_VERSION}/system_core.dic config/sudachi/system_core.dic
RUN /usr/share/opensearch/bin/opensearch-plugin install --batch file:opensearch-${OPENSEARCH_TAG}-analysis-sudachi-${SUDACHI_PLUGIN_VERSION}.zip

# Install plugins
RUN if [ "${INSTALL_PLUGIN_ANALYSIS_PHONETIC}" = "true" ]; then bin/opensearch-plugin install analysis-phonetic; fi
RUN if [ "${INSTALL_PLUGIN_ANALYSIS_ICU}" = "true" ]; then bin/opensearch-plugin install analysis-icu; fi
