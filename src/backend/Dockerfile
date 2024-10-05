FROM node:lts-buster-slim AS development

WORKDIR /usr/src/app

COPY package*.json ./ 
RUN npm ci

COPY . .

# Run vulnerability scan on build image
FROM build AS vulnscan
COPY --from=aquasec/trivy:latest /usr/local/bin/trivy /usr/local/bin/trivy
RUN trivy rootfs --no-progress /

# Production stage (no build step here)
FROM node:alpine AS production

WORKDIR /usr/src/app

COPY --from=development /usr/src/app . 

EXPOSE 3000

CMD [ "npm", "run", "dev" ]