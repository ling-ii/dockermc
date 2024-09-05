#
#           STAGE 1
#

FROM eclipse-temurin:22-alpine as jre-build

# COPY fabric-server-mc.1.20.4-loader.0.15.9-launcher.1.0.0.jar /minecraft

# RUN jdeps \
#     --ignore-missing-deps \
#     --print-module-deps \
#     --recursive \
#     --class-path "BOOT-INF/lib/*" \
#     fabric-server-mc.1.20.4-loader.0.15.9-launcher.1.0.0.jar > modules.txt
#

RUN $JAVA_HOME/bin/jlink \
    --add-modules java.base,java.logging,java.desktop,java.sql,java.management,jdk.zipfs,java.compiler\
    --strip-debug \
    --no-man-pages \
    --no-header-files \
    --compress=2 \
    --output /jre

#
#           STAGE 2
#

FROM alpine:latest

WORKDIR /minecraft

ENV JAVA_HOME=/opt/java/openjdk
ENV PATH "${JAVA_HOME}/bin:${PATH}"

#COPY --from=jre-build /minecraft/modules.txt /minecraft

COPY ./start.sh /minecraft
COPY ./fabric-server-mc.1.20.6-loader.0.15.10-launcher.1.0.1.jar /minecraft
COPY ./lithium-fabric-mc1.20.6-0.12.3.jar /minecraft/mods/
COPY --from=jre-build /jre ${JAVA_HOME}

RUN echo "eula=true" > eula.txt

EXPOSE 25565

ENTRYPOINT [ "sh" ]