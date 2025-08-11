# =========================================================================
#  第一阶段 (Builder Stage): 使用一个功能齐全的镜像来编译和打包 Java 应用
# =========================================================================
FROM maven:3.9.6-eclipse-temurin-17-alpine AS builder

# 设置工作目录
WORKDIR /app

# 1. 单独复制 pom.xml
COPY ./pom.xml .

# 2. 下载所有依赖项
RUN mvn dependency:go-offline

# 3. 复制所有源代码
COPY ./src ./src

# 4. 执行 Maven 打包命令 (这一步会生成 target/youlai-boot.jar)
RUN mvn -B package -DskipTests


# =========================================================================
#  第二阶段 (Final Stage): 使用一个极简的镜像来运行应用
# =========================================================================
FROM eclipse-temurin:17-jre-alpine

LABEL maintainer="youlai <youlaitech@163.com>"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 设置工作目录
WORKDIR /app

# 从第一阶段 (builder) 中，只复制最终生成的那个 JAR 文件。
COPY --from=builder /app/target/youlai-boot.jar .

# 暴露端口
EXPOSE 8989

# 容器启动时运行的命令
ENTRYPOINT ["java", "-Xms512m", "-Xmx512m", "-Djava.security.egd=file:/dev/./urandom", "-jar", "youlai-boot.jar"]