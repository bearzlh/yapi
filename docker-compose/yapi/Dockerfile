FROM node:12.22.12-alpine3.15

#yapi版本号
ARG VERSION=1.10.2
#暴露的端口
ARG WEBPORT=3000
#管理员账号
ARG ADMIN_ACCOUNT=admin@admin.com
#mongodb ip或主机名
ARG SERVERNAME=192.168.8.54
#mongodb dbname
ARG DATABASE=yapi
#mongodb 端口
ARG PORT=27017
ARG USER
ARG PASS
ARG INIT
#是否需要执行初始化
ENV INIT $INIT
ENV WEBPORT $WEBPORT

RUN echo -e "http://mirrors.aliyun.com/alpine/v3.15/main/\nhttp://mirrors.aliyun.com/alpine/v3.15/community/" > /etc/apk/repositories && \
  apk add --no-cache wget openssl jq && \
  mkdir vendors && wget https://github.com/YMFE/yapi/archive/refs/tags/v${VERSION}.tar.gz && tar zxf v${VERSION}.tar.gz -C vendors --strip-components 1 && \
  echo '{"webport":"'$WEBPORT'","adminAccount":"'$ADMIN_ACCOUNT'","servername":"'$SERVERNAME'","DATABASE":"'$DATABASE'","port":"'$PORT'","user":"'$USER'","pass":"'$PASS'"}' | jq '{"port":.webport,"adminAccount":.adminAccount,"timeout":120000,"db":{"servername":.servername,"DATABASE":.DATABASE,"port":.port,"user":.user,"pass":.pass,"authSource":""},"mail":{"enable":false,"host":"smtp.163.com","port":465,"from":"***@163.com","auth":{"user":"***@163.com","pass":"*****"}}}' > /config.json && \
  apk del wget openssl jq

WORKDIR /vendors

EXPOSE $WEBPORT

#如果mongodb未初始化，请设置INIT非空
ENTRYPOINT ["sh","-c", "([ ' '$INIT != ' ' ] && cd /vendors/ && npm run install-server);cd /vendors && rm package-lock.json && npm install --registry https://registry.npm.taobao.org; cd / && node /vendors/server/app.js"]