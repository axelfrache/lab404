FROM dhi.io/python:3.14-dev@sha256:42fb1e613a3fd86e85c30f1b054536ba642c455123d6b4bcbd796aa49de4e46b AS build

WORKDIR /app
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
COPY requirements.txt .
RUN pip install --no-cache-dir --no-compile --require-hashes -r requirements.txt \
    && pip uninstall -y pip

FROM dhi.io/python:3.14@sha256:aa13af4b3777149bbf18dc6fe392c5ab9eb097d39715d57f4bacde537b6e0e26

WORKDIR /app
ENV PATH="/opt/venv/bin:$PATH" \
    OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1
COPY --from=build /opt/venv /opt/venv
COPY app.py gunicorn.conf.py ./
COPY templates templates

USER 65532:65532
EXPOSE 8080
CMD ["opentelemetry-instrument", "gunicorn", "app:app"]
