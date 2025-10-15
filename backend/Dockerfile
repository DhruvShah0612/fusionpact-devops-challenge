# ==========================
# Backend - FastAPI + MySQL
# ==========================
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install system dependencies and copy files
RUN apt-get update && apt-get install -y default-libmysqlclient-dev build-essential

# Copy requirements file if available
COPY backend/requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy backend source code
COPY backend/ .

# Expose FastAPI port
EXPOSE 8000

# Run FastAPI app using uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
