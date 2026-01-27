# ---- Build stage ----
FROM node:16 AS build
WORKDIR /app

# Copy package files first (cache-friendly)
COPY package.json package-lock.json ./

# Reproducible install (uses package-lock)
RUN npm ci

# Copy the rest of the app
COPY . .

# Build static assets
RUN npm run build


# ---- Run stage ----
FROM node:18-slim
WORKDIR /app

# Static server
RUN npm install -g serve

# Only copy build output
COPY --from=build /app/build ./build

EXPOSE 3000
CMD ["serve", "-s", "build", "-l", "3000"]
