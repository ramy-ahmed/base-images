# AUTOGENERATED FILE
FROM balenalib/npe-x500-m3-debian:stretch-run

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        \
# .NET Core dependencies
        libc6 \
        libgcc1 \
        libgssapi-krb5-2 \
        libicu57 \
        libssl1.1 \
        libstdc++6 \
        zlib1g \
    && rm -rf /var/lib/apt/lists/*

# Configure web servers to bind to port 80 when present
ENV ASPNETCORE_URLS=http://+:80 \
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINER=true

# Install .NET Core
ENV DOTNET_VERSION 3.1.5

RUN curl -SL --output dotnet.tar.gz "https://dotnetcli.blob.core.windows.net/dotnet/Runtime/$DOTNET_VERSION/dotnet-runtime-$DOTNET_VERSION-linux-arm.tar.gz" \
    && dotnet_sha512='c3eb0453893ca7f758e491780031ef0b1323a14c080740847aea652b5c4db99d30e8b3b27fcd306c3a098dc838572c41d3ea871156ba62d9302df32e63a28835' \
    && echo "$dotnet_sha512 dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -zxf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

ENV ASPNETCORE_VERSION 3.1.5

RUN curl -SL --output aspnetcore.tar.gz "https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$ASPNETCORE_VERSION/aspnetcore-runtime-$ASPNETCORE_VERSION-linux-arm.tar.gz" \
    && aspnetcore_sha512='090ef0da2454935015184f4b4b968c049eb806d19dcd258af06265e922f9d0df209fa9f84ebefe9e7cb6bb391dd0459eb3b5f7e8609fca2cffd75d3801c1b6a7' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -zxf aspnetcore.tar.gz -C /usr/share/dotnet ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz

CMD ["echo","'No CMD command was set in Dockerfile! Details about CMD command could be found in Dockerfile Guide section in our Docs. Here's the link: https://balena.io/docs"]

 RUN curl -SLO "https://raw.githubusercontent.com/balena-io-library/base-images/8accad6af708fca7271c5c65f18a86782e19f877/scripts/assets/tests/test-stack@dotnet.sh" \
  && echo "Running test-stack@dotnet" \
  && chmod +x test-stack@dotnet.sh \
  && bash test-stack@dotnet.sh \
  && rm -rf test-stack@dotnet.sh 

RUN [ ! -d /.balena/messages ] && mkdir -p /.balena/messages; echo 'Here are a few details about this Docker image (For more information please visit https://www.balena.io/docs/reference/base-images/base-images/): \nArchitecture: ARM v7 \nOS: Debian Stretch \nVariant: run variant \nDefault variable(s): UDEV=off \nThe following software stack is preinstalled: \ndotnet 3.1-aspnet \nExtra features: \n- Easy way to install packages with `install_packages <package-name>` command \n- Run anywhere with cross-build feature  (for ARM only) \n- Keep the container idling with `balena-idle` command \n- Show base image details with `balena-info` command' > /.balena/messages/image-info

RUN echo '#!/bin/sh.real\nbalena-info\nrm -f /bin/sh\ncp /bin/sh.real /bin/sh\n/bin/sh "$@"' > /bin/sh-shim \
	&& chmod +x /bin/sh-shim \
	&& cp /bin/sh /bin/sh.real \
	&& mv /bin/sh-shim /bin/sh