FROM debian:stable-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && \
    apt install -y \
      reprepro \
      nginx \
      ca-certificates \
      dpkg-dev \
    && rm -rf /var/lib/apt/lists/*

# Directorio del repositorio (volumen)
RUN mkdir -p /repo/conf

# ---------- CREAR PAQUETE .DEB ----------
WORKDIR /build

RUN mkdir -p paqprueba/DEBIAN \
             paqprueba/usr/bin

# Control del paquete
RUN printf "Package: paqprueba\n\
Version: 1.0\n\
Section: base\n\
Priority: optional\n\
Architecture: amd64\n\
Maintainer: Yo <yo@local>\n\
Description: Paquete de prueba que hace echo\n" \
> paqprueba/DEBIAN/control

# Script ejecutable
RUN printf "#!/bin/sh\n\
echo \"Hola desde paqprueba. Si lees esto es que todo ha funcionado :)\"\n" \
> paqprueba/usr/bin/paqprueba && \
chmod +x paqprueba/usr/bin/paqprueba

# Construir el .deb
RUN dpkg-deb --build paqprueba

# ---------- CONFIGURAR REPO (SIN GPG) ----------
WORKDIR /repo

RUN cat > /repo/conf/distributions <<'EOF'
Origin: MiRepo
Label: MiRepo
Suite: stable
Codename: stable
Architectures: amd64
Components: main
Description: Repo Debian sin GPG
EOF


# Importar paquete al repo
RUN reprepro includedeb stable /build/paqprueba.deb

# ---------- NGINX ----------
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
