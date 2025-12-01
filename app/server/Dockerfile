# Use Node.js 18 Alpine as base image
FROM node:18-alpine

# Install dependencies for health checking
RUN apk add --no-cache curl

# Create app directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies (clean install)
RUN npm ci --only=production && \
    npm cache clean --force

# Create non-root user
RUN adduser --random-password --disabled-password --gecos "" --home /home/appuser --shell /bin/sh appuser

# Copy application code
COPY --chown=appuser:appuser . .

# Switch to non-root user
USER appuser

# Expose port
EXPOSE 3000

# Add health check
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:3000/health || exit 1

# Start the application
CMD ["node", "server.js"]