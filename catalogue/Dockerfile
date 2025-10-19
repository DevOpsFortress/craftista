# Build stage
FROM python:3.11-slim AS builder

WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir --user -r requirements.txt

# Runtime stage
FROM gcr.io/distroless/python3-debian12

WORKDIR /app

# Copy Python packages from builder
COPY --from=builder /root/.local /root/.local
ENV PATH=/root/.local/bin:$PATH

# Copy application code
COPY app.py .
COPY products.json .
COPY config.json .
COPY templates/ templates/
COPY static/ static/

# Expose port
EXPOSE 5000

# Run with gunicorn
CMD ["-m", "gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
