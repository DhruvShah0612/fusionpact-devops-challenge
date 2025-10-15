FROM python:3.11-slim

WORKDIR /app

RUN apt-get update && apt-get install -y default-libmysqlclient-dev build-essential

# Copy requirements (no 'backend/' here)
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

# Copy app folder (no 'backend/' here)
COPY app ./app

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
