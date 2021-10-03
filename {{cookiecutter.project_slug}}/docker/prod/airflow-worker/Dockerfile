ARG AIRFLOW_TAG="2.0.1-debian-10-r46"

FROM docker.io/bitnami/airflow-worker:${AIRFLOW_TAG}

COPY airflow/requirements/* /bitnami/python/

RUN echo "Checking for requirements.txt in /bitnami/python" \
        ; if [ -f "bitnami/python/requirements.txt" ] ; then \
        echo "Installing packages" && \
        . /opt/bitnami/airflow/venv/bin/activate && \
        pip install -r /bitnami/python/requirements.txt \
        ; fi

COPY airflow/plugins /opt/bitnami/airflow/plugins

COPY airflow/dags /opt/bitnami/airflow/dags