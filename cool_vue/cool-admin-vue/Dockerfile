FROM node:lts-alpine
WORKDIR /build
# 设置npm镜像
RUN npm config set registry https://registry.npmmirror.com
COPY package.json /build/package.json
RUN npm install
COPY ./ /build
RUN npm run build

FROM nginx
RUN mkdir /app
COPY --from=0 /build/dist /app
COPY --from=0 /build/nginx.conf /etc/nginx/nginx.conf
EXPOSE 9000
# 暴露前端端口（和 docker-compose 一致）
# 启动 Nginx
CMD ["nginx", "-g", "daemon off;"]