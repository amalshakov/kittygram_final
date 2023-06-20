FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt ./

RUN pip install -r requirements.txt --no-cache-dir

RUN mkdir backend_static/static

COPY . .

CMD ["gunicorn", "--bind", "0.0.0.0:9000", "kittygram_backend.wsgi"]
