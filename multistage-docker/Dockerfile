# Install dependencies only when needed
# Stage 1 -> We create an image with all the nodejs dependencies
FROM node:12-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install --frozen-lockfile


# Rebuild the source code only when needed
# Stage 2 -> We create a docker to build the project 
# We recopy all the downloaded nodejs mdodule from the docker image deps
FROM node:12-alpine AS builder
WORKDIR /app
COPY . .
COPY --from=deps /app/node_modules ./node_modules
RUN npm run build 



# Production image, copy all the files and run 
FROM node:12-alpine AS runner
WORKDIR /app

#ENV NODE_ENV production

COPY --from=builder /app/dist .
COPY --from=builder /app/node_modules ./node_modules

RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodeapp -u 1001
RUN chown -R nodeapp:nodejs /app/
USER nodeapp

EXPOSE 8080

# Next.js collects completely anonymous telemetry data about general usage.
# Learn more here: https://nextjs.org/telemetry
# Uncomment the following line in case you want to disable telemetry.
# RUN npx next telemetry disable

CMD ["node", "/app/index.js"]