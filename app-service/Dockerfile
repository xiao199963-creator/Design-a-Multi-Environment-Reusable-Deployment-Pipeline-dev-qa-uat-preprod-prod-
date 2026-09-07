FROM python:3.12-slim AS builder
WORKDIR /build
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

FROM python:3.12-slim AS runtime
WORKDIR /app
RUN groupadd --system app && useradd --system --gid app app
COPY --from=builder /install /usr/local
COPY --chown=app:app app.py .
USER app
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3  \
    CMD [ "python", "-c","import urllib.request; urllib.request.urlopen('http://127.0.0.1:8000/healthz', timeout=2)" ]
CMD [ "gunicorn","--bind","0.0.0.0:8000","app:app" ]