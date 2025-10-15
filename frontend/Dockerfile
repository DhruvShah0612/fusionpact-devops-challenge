# ==========================
# Frontend - React/Vite/HTML
# ==========================
FROM node:20-alpine AS build

# Set working directory
WORKDIR /app

# Copy frontend source code
COPY frontend/ .

# Install dependencies and build
RUN npm install && npm run build

# ==========================
# Serve using Nginx
# ==========================
FROM nginx:alpine

# Copy built frontend to Nginx html directory
COPY --from=build /app/dist /usr/share/nginx/html

# Expose frontend port
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]
