FROM node:20-alpine AS build
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN npm install -g pnpm && pnpm install --frozen-lockfile
COPY . .
RUN pnpm run build
# Move the output to a predictable location, supporting different Angular versions
RUN mkdir -p dist/final_output && \
    (mv dist/frontend/browser/* dist/final_output/ 2>/dev/null || \
     mv dist/frontend/* dist/final_output/ 2>/dev/null || \
     mv dist/* dist/final_output/ 2>/dev/null)

FROM nginx:alpine
COPY --from=build /app/dist/final_output /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 8080
