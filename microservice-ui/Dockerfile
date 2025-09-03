# ---- Build stage ----
FROM node:18 AS builder
WORKDIR /app

# Fix OpenSSL 3 + webpack hashing issue
ENV NODE_OPTIONS=--openssl-legacy-provider

# Install deps first (cache-friendly)
COPY package*.json ./
RUN npm ci --no-audit --no-fund

# Copy the rest and build
COPY . .
# If your package.json has "build": "ng build", this will run Angular prod build
RUN npm run build -- --configuration=production --output-path=dist

# ---- Runtime (nginx) ----
FROM nginx:alpine
# Optional: custom SPA config so deep-links resolve to index.html
COPY nginx.conf /etc/nginx/nginx.conf
# Copy the built app
COPY --from=builder /app/dist/ /usr/share/nginx/html
