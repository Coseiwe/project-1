# Stage 1: dependebcy installation and build stage
FROM python:3.10-alpine AS builder

# Install system dependencies
RUN apk add --no-cache gcc musl-dev libffi-dev postgresql-dev

# Install Poetry
RUN pip install --no-cache-dir poetry

WORKDIR /app

COPY pyproject.toml poetry.lock ./

# Install dependencies into a virtual environment
RUN poetry config virtualenvs.create false && poetry install --no-dev --no-interaction --no-ansi

COPY . .

# Stage 2: Runtime image
FROM python:3.10-alpine

# Install runtime dependencies
RUN apk add --no-cache libpq

WORKDIR /app

# Copy the built app and dependencies
COPY --from=builder /app /app

# Copy the prestart script
COPY ./prestart.sh /prestart.sh

# Expose application port
EXPOSE 8000

# Run the app
CMD ["sh", "-c", "bash /prestart.sh && uvicorn app.main:app --host 0.0.0.0 --port 8000"]
