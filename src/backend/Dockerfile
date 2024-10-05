FROM node:20-slim AS development

WORKDIR /usr/src/app

COPY package*.json ./ 
RUN npm ci

COPY . .

# Production stage (no build step here)
FROM node:alpine AS production

WORKDIR /usr/src/app

COPY --from=development /usr/src/app . 

EXPOSE 3000

CMD [ "npm", "run", "dev" ]