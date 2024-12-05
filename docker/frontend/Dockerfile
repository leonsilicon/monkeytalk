FROM node:20.16.0-alpine3.19 AS builder
WORKDIR /app

#ENV
ENV BACKEND_URL=https://backend.monkeytalk.xyz RECAPTCHA_SITE_KEY=6LcYipMqAAAAAKhAV-E-eVGmcmLNE4nQ7rIh9gjf

#COPY
COPY ["package.json", "pnpm-lock.yaml", "pnpm-workspace.yaml", "turbo.json", "./"]
COPY packages packages
COPY frontend frontend

COPY docker/frontend/firebase-config-live.ts frontend/src/ts/constants/firebase-config.ts
COPY docker/frontend/firebase-config-live.ts frontend/src/ts/constants/firebase-config-live.ts

#gimme pnpm + build
RUN npm i -g pnpm && \
    pnpm i --frozen-lockfile && \
    npm run build

# COPY to target
FROM nginx:mainline-alpine

COPY --from=builder /app/frontend/dist /usr/share/nginx/html
COPY docker/frontend/updateConfig.sh /docker-entrypoint.d/updateConfig.sh
RUN chmod +x /docker-entrypoint.d/updateConfig.sh

# entry
#CMD ["./entryPoint.sh"]
