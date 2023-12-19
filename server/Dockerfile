FROM python:3.11-slim AS build

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PATH="/app/.venv/bin:$PATH" \
    TZ=Europe/Moscow

RUN groupadd -g 1000 user && useradd -u 1000 --gid user -m -d /app --shell /bin/bash user
RUN mkdir -p /app && chown user:user /app
RUN mkdir -p /opt/poetry && chown user:user /opt/poetry

USER user:user

RUN python -m venv /opt/poetry
RUN /opt/poetry/bin/pip install --no-cache-dir --progress-bar off poetry

WORKDIR /app
COPY --chown=user:user poetry.toml poetry.lock pyproject.toml .
RUN /opt/poetry/bin/poetry install --only main


FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PATH="/app/.venv/bin:$PATH" \
    TZ=Europe/Moscow

RUN groupadd -g 1000 user && useradd -u 1000 --gid user -m -d /app --shell /bin/bash user
RUN mkdir -p /app && chown user:user /app

USER user:user

EXPOSE 8001

WORKDIR /app

COPY --from=build --chown=user:user /app/.venv /app/.venv
COPY --chown=user:user ./manage.py ./manage.py
COPY --chown=user:user ./base ./base
COPY --chown=user:user ./labres ./labres

RUN ["./manage.py", "collectstatic", "--noinput"]

CMD ["gunicorn", "--bind", ":8001", "base.wsgi:application"]
