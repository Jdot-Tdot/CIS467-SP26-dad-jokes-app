# ---- Build Stage ----
FROM node:20-alpine as base
WORKDIR /app

# Copy dependency manifests first (layer caching)
# package* gets both package.json and package-lock.json (if it exists)
COPY package*.json ./

FROM base as dev
RUN npm install
COPY . .
CMD ["npm", "run", "dev"]

# Copy source and compile TypeScript
FROM base AS build
RUN npm ci
COPY tsconfig.json ./
COPY src ./src
RUN npm run build

# ---- Production Stage ----
FROM node:20-alpine AS prod

WORKDIR /app

COPY package*.json ./
RUN npm ci --omit=dev

# Copy compiled JS from builder
COPY --from=build /app/dist ./dist

EXPOSE 3000
CMD ["node", "dist/index.js"]