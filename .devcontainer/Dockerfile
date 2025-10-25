FROM n8nio/base:22

RUN apk add --no-cache --update openssh sudo shadow bash
RUN echo node ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/node && chmod 0440 /etc/sudoers.d/node
RUN mkdir /workspaces && chown node:node /workspaces
RUN npm install -g pnpm

USER node
RUN mkdir -p ~/.pnpm-store && pnpm config set store-dir ~/.pnpm-store --global

# Копируем package.json и lock-файлы
COPY --chown=node:node package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY --chown=node:node packages ./packages

# Устанавливаем зависимости
RUN pnpm install --frozen-lockfile

# ТЕПЕРЬ пересобираем sharp для musl
RUN pnpm rebuild sharp --arch=x64 --platform=linux --libc=musl

# Копируем остальные файлы проекта
COPY --chown=node:node . .

CMD ["pnpm", "start"]
