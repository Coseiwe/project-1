FROM python:3.10-alpine AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip wheel --no-cache-dir --wheel-dir=/wheels -r requirements.txt


FROM python:3.10-alpine
WORKDIR /app
COPY --from=builder /wheels /wheels
RUN pip install --no-cache-dir --find-links=/wheels -r /wheels/*
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
