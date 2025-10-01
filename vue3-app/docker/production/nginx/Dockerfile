# ==============================================================================
# Nginx Stage - Serve static files
# ==============================================================================
FROM nginx:alpine

# Remove default nginx configuration
RUN rm /etc/nginx/conf.d/default.conf

# Copy custom nginx configuration
COPY docker/production/nginx/nginx.conf /etc/nginx/conf.d/default.conf

# The built files will be mounted as a volume at /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
