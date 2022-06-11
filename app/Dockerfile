FROM python:3.8-slim as requirements-stage
WORKDIR /tmp
RUN pip install peotry
COPY ../pyproject.toml ../peotry.lock* /tmp/
RUN poetry export -f requirements.txt --output requirements.txt --without-hashes

FROM python:3.8-slim
WORKDIR /app
COPY --from=requirements-stage /tmp/requirements.txt /app/requirements.txt

RUN apt update && \
    apt install gcc g++ -y && \
    pip install --upgrade pip && \
    pip install --no-cache-dir --upgrade -r /app/requirements.txt

COPY . /app/
COPY ../medical_ner /app/

CMD ["uvicorn", "api:app", "--host", "0.0.0.0", "--port", "8080"]