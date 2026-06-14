# Ada
Bot de Telegram que permite generar notificaciones usando una API REST y un cliente BASH

![ada-siiga](https://github.com/user-attachments/assets/445aa91a-0115-4c9c-94cf-65eee431b8c3)

- Envía mensajes a través de una API REST
- Cliente BASH para enviar mensajes desde scripts
- Edita mensajes ya enviados
- Panel administrativo para crear topics y ver logs

## Instalación usando [uv](https://astral.sh/blog/uv)
```bash
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

## Correr localmente
El mismo programa te guiará para la instalación inicial.
```bash
source .venv/bin/activate
python3 main.py
```

## Correr en la nube (PM2)
Instalar Node y PM2
```bash
curl -fsSL https://fnm.vercel.app/install | bash
source ~/.bashrc
fnm install --lts
npm i -g pm2
```
Levantar con PM2
```bash
pm2 start -n ada "python main.py"
```
Configurar servidor web con Nginx Proxy
1. [Instalar docker en Debian](https://docs.docker.com/engine/install/debian/)
2. [Instalar Nginx Proxy](https://www.notion.so/opaito/docker-nginx-proxy-manager-ui-1213b18d779880b78ffdceeef72c3dca)

---

## Panel administrativo
El panel administrativo se encuentra en [http://localhost:3839](http://localhost:3839) (o cualquier otra URL pública que configures).

Debes realizar la configuración inicial antes de poder usarlo. Simplemente ejecuta el programa y sigue las instrucciones.

## Mensaje rápido
Puedes crear un topic llamado `ejemplo` y enviar un mensaje rápido con CURL o clickeando [aquí](http://localhost:3839/ejemplo/hola).
```
curl http://localhost:3839/ejemplo/hola
```

## Cliente BASH
Se incluye el script `ada.sh` que es un cliente 100% stand-alone y se puede copiar a cualquier proyecto para enviar mensajes al bot.

Este script permite editar un mensaje ya enviado, útil para no llenar el chat de mensajes sobre un mismo proceso.

```bash
./ada.sh [topic] [mensaje] [editar_id]
```

El script finalizará mostrando  el `id` del mensaje, para que podamos encadenarlo. 

### Ejemplo de uso del cliente en un script de respaldo:

```bash
# notificación de Ada
ada_topic="respaldos"
ada_message_id=
function ada_log {
  if [ ! -f "ada.sh" ]; then
    echo "Cliente de Ada no encontrado"
    exit 1
  fi
  ada_message_id=$(./ada.sh "$ada_topic" "$1" $ada_message_id)
}

# enviar mensaje inicial
ada_log "Iniciando respaldo"

# actualizar mensaje
ada_log "Respaldo terminado"
```

## Comandos Telegram

| Comando | Descripción                                                                 |
|---------|-----------------------------------------------------------------------------|
| `/start` | Inicializa el bot. Si no existen usuarios en la DB, guardará al usuario como administrador |
| `/panel` | Muestra un botón que permite acceder directamente al panel administrativo |

