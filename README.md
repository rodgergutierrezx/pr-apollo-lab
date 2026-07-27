# AI Server CPU

Servidor de desarrollo para IA con:

- Docker Compose
- Conda/Miniforge
- Python 3.12
- PyTorch para CPU
- JupyterLab
- Transformers y herramientas comunes de IA
- Caddy como proxy HTTPS

## 1. Requisitos

- Linux con Docker Engine
- Docker Compose
- Un dominio o subdominio apuntando a la IP pública del servidor
- Puertos TCP 80 y 443 abiertos
- Puerto UDP 443 opcional para HTTP/3

La máquina actual no necesita GPU NVIDIA.

## 2. Configuración

Copia el archivo de ejemplo:

```bash
cp .env.example .env
```

Edita `.env`:

```env
JUPYTER_DOMAIN=jupyter.tudominio.com
JUPYTER_TOKEN=un-token-muy-largo-y-aleatorio
```

Puedes generar un token con:

```bash
openssl rand -hex 32
```

## 3. DNS

Crea un registro DNS tipo `A`:

```text
jupyter.tudominio.com -> IP_PUBLICA_DEL_SERVIDOR
```

## 4. Iniciar

```bash
docker compose up -d --build
```

Ver registros:

```bash
docker compose logs -f
```

## 5. Acceder

Desde la Mac:

```text
https://jupyter.tudominio.com
```

Introduce el valor de `JUPYTER_TOKEN`.

## 6. Comprobar PyTorch

Abre:

```text
workspace/notebooks/00_verificacion.ipynb
```

En esta versión debe mostrar:

```text
CUDA disponible: False
Dispositivo: cpu
```

## 7. Apagar

```bash
docker compose down
```

Para borrar también los volúmenes internos de Caddy:

```bash
docker compose down -v
```

## 8. Seguridad

Jupyter no publica el puerto 8888 hacia Internet. Solo Caddy puede comunicarse con él mediante la red privada de Docker.

No publiques `8888:8888`.

No montes `/var/run/docker.sock` ni la raíz `/` dentro del contenedor.

Mantén actualizado el servidor:

```bash
sudo apt update
sudo apt upgrade
docker compose pull
docker compose up -d --build
```

## 9. Migración futura a CUDA

Cuando instales una GPU NVIDIA:

1. Instala el controlador NVIDIA en el host.
2. Instala NVIDIA Container Toolkit.
3. Crea un Dockerfile basado en `nvidia/cuda`.
4. Instala la variante CUDA de PyTorch.
5. Agrega `gpus: all` al servicio `jupyter`.

La carpeta `workspace` y los notebooks no necesitan modificarse.
