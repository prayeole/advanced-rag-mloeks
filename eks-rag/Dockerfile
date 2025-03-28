FROM python:3.9-slim

WORKDIR /app

# Install AWS CLI dependencies if needed
RUN apt-get update && apt-get install -y \
    curl \
    unzip \
    less \
    groff \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm -rf aws awscliv2.zip \
    && rm -rf /var/lib/apt/lists/*


COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Ensure proper permissions for the application user
RUN adduser --disabled-password --gecos '' appuser
RUN chown -R appuser:appuser /app
USER appuser

EXPOSE 5000

CMD ["gunicorn", "--timeout", "120", "--workers", "2", "--bind", "0.0.0.0:5000", "vector_search_service:app"]
