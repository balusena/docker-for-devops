FROM python:3.9

LABEL maintainer="balasenapathi@example.com" \
      version="1.0" \
      description="A Python Flask application"

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    ENVIRONMENT=development

ARG APP_VERSION=1.0

WORKDIR /usr/src/app

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY . /usr/src/app

EXPOSE 5000

VOLUME ["/usr/src/app/logs"]

HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:5000/ || exit 1

CMD ["python", "app.py"]

LABEL build_version="${APP_VERSION}"
