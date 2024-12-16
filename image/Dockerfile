# Use the official Airflow image as a parent image
FROM apache/airflow:2.9.1-python3.11

USER root

ARG CLOUD_SDK_VERSION=426.0.0
ENV GCLOUD_HOME=/opt/google-cloud-sdk
ENV PATH="${GCLOUD_HOME}/bin/:${PATH}"

## default install google cloud sdk
RUN DOWNLOAD_URL="https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz" \
  && TMP_DIR="$(mktemp -d)" \
  && curl -fL "${DOWNLOAD_URL}" --output "${TMP_DIR}/google-cloud-sdk.tar.gz" \
  && mkdir -p "${GCLOUD_HOME}" \
  && tar xzf "${TMP_DIR}/google-cloud-sdk.tar.gz" -C "${GCLOUD_HOME}" --strip-components=1 \
  && "${GCLOUD_HOME}/install.sh" \
  --bash-completion=false \
  --path-update=false \
  --usage-reporting=false \
  --additional-components alpha beta kubectl \
  --quiet \
  && rm -rf "${TMP_DIR}" \
  && rm -rf "${GCLOUD_HOME}/.install/.backup/" \
  && gcloud --version

## kubectl installation
ENV KUBECTL_VERSION "v1.25.2"
RUN curl -L -o /usr/local/bin/kubectl "https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl"
RUN chmod +x /usr/local/bin/kubectl

WORKDIR /var/airflow

COPY requirement.txt /var/airflow/requirements.txt

# Create plugins and dags directory
RUN mkdir -p /var/airflow/plugins

# Create plugins and dags directory
RUN chmod -R 777 /var/airflow/plugins

# Copy plugins and dags to folders
COPY plugin/. /var/airflow/plugins/

USER airflow

# Install the necessary dependencies
RUN pip install \
    --no-cache-dir \
    --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.9.1/constraints-3.11.txt" \
    "apache-airflow==2.9.1" -r /var/airflow/requirements.txt