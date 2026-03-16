# ---- Build Stage ----
FROM node:20-alpine as build

WORKDIR /app

# Copy dependency manifests first (layer caching)
# package* gets both package.json and package-lock.json (if it exists)
COPY package*.json ./
RUN npm install

# Copy source and compile TypeScript
COPY tsconfig.json ./
COPY src ./src
RUN npm run build

# ---- Production Stage ----
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install --omit=dev

# Copy compiled JS from builder
COPY --from=build /app/dist ./dist

EXPOSE 3000
CMD ["node", "dist/index.js"]