FROM nginx:alpine

# Remove default nginx html
RUN rm -rf /usr/share/nginx/html/*

# Copy everything in current folder (frontend/) to nginx html folder
COPY . /usr/share/nginx/html/

# Rename the file to index.html safely
RUN [ -f /usr/share/nginx/html/Devops_Intern.html ] && mv /usr/share/nginx/html/Devops_Intern.html /usr/share/nginx/html/index.html || true

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
