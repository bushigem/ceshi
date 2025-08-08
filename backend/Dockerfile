# -------------- 基础阶段 --------------
FROM openjdk:17

LABEL maintainer="youlai <youlaitech@163.com>"

# 时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 匿名卷
VOLUME /tmp

# 构建参数：jar 文件名
ARG JAR_FILE=target/youlai-boot.jar

# 复制并改名
COPY ${JAR_FILE} /app.jar

EXPOSE 8989
ENTRYPOINT ["java", "-Xms512m", "-Xmx512m", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app.jar"]