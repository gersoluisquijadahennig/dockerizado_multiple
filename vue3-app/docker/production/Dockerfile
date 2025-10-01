# ==============================================================================
# Builder Stage - Build the Vue 3 application
# ==============================================================================
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci --only=production=false

# Copy application source code
COPY . .

# Build the application for production
RUN npm run build

# The dist folder will be copied to a volume, so other services can use it

# ==============================================================================
# Production Stage - Serve with Node.js (optional, for preview)
# ==============================================================================
FROM node:20-alpine AS production

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install only production dependencies
RUN npm ci --only=production

# Copy built assets from builder stage
COPY --from=builder /app/dist ./dist

# Expose preview port
EXPOSE 3000

# Start preview server
CMD ["npm", "run", "preview"]
