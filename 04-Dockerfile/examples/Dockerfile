# Use the official Python image from the Docker Hub
FROM python:3.9

# Adds metadata to the image, such as maintainer information or build details.
LABEL maintainer="balasenapathi@example.com" \
      version="1.0" \
      description="A Python Flask application"

# Define environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    ENVIRONMENT=development

# Define build argument
ARG APP_VERSION=1.0

# Set working directory
WORKDIR /usr/src/app

# Copy and install dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy the application code
COPY . /usr/src/app

# Expose application port
EXPOSE 5000

# Define volume for logs
VOLUME ["/usr/src/app/logs"]

# Set the user (ensure this user exists or remove this line)
# USER balu

# Health check
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:5000/ || exit 1

# Set default command
CMD ["python", "app.py"]

# Label with build version
LABEL build_version="${APP_VERSION}"
