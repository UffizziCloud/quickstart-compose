# Using official python runtime base image
FROM python:3.9-slim

# add curl for healthcheck
# add wait-for-it to delay start
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    wait-for-it \
    && rm -rf /var/lib/apt/lists/*

# Set the application directory
WORKDIR /app

# Install our requirements.txt
COPY requirements.txt /app/requirements.txt
RUN pip install -r requirements.txt

# Copy our code from the current folder to /app inside the container
COPY . .

# Make port 8888 available for links and/or publish
EXPOSE 8888

ENV REDIS_HOST=localhost

# Define our command to be run when launching the container
CMD ["sh", "-c", "wait-for-it --strict --timeout=60 ${REDIS_HOST}:6379 -- gunicorn app:app -b 0.0.0.0:8888 --log-file - --access-logfile - --workers 4 --keep-alive 0"]
