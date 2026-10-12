# --- Build Stage ---
FROM node:18-alpine AS builder

WORKDIR /app

# Copy dependency manifests
COPY package*.json ./

# Install all dependencies (including devDependencies for compilation)
RUN npm install

# Copy configuration files
COPY tsconfig.json ./

# Copy your source code (assumes server.ts lives inside src/)
COPY src/ ./src/

# Compile TypeScript to JavaScript
RUN npm run build

# --- Production Stage ---
FROM node:18-alpine AS runner

WORKDIR /app

# Set production environment
ENV NODE_ENV=production

# Copy built app and dependency manifests
COPY package*.json ./
COPY --from=builder /app/dist ./dist

# Install only production dependencies
RUN npm ci --only=production

# Expose port (adjust if your .env uses a different port)
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
