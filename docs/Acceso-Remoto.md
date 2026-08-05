# Tailscale — Acceso remoto

En esta fase de servidor de acceso local, no es posible trabajar con el servidor de forma remota mediante SSH por IP, ya que no hay IP pública, solo funciona dentro de la red local, para que los administradores puedan conectarse remotamente se puede utilizar el servicio que Tailscale, existen otras alternativas pero esta es la más sencilla de instalar y utilizar.

## 1. Instalar Tailscale en el servidor

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

## 2. Levantar el servicio y autenticarse

```bash
sudo tailscale up
```

Dvuelve una URL — ábrela en cualquier navegador (de tu laptop, celular, donde sea) y autentícate con una cuenta (Google, GitHub, Microsoft, o email). Esto vincula el servidor a tu "tailnet" (tu red privada de Tailscale).

## 3. Habilitar acceso a la red local

Por defecto Tailscale solo da acceso al servidor mismo. Para llegar también a Moodle/otros equipos de la red `192.168.100.0/24` desde afuera, el servidor debe actuar como **subnet router**:

```bash
sudo tailscale up --advertise-routes=192.168.100.0/24 --accept-dns=false
```

## 4. Habilitar IP forwarding en el servidor (requisito para subnet router)

```bash
sudo nano /etc/sysctl.conf
```

Agregar o descomentar:

```
net.ipv4.ip_forward=1
```

Aplicar:

```bash
sudo sysctl -p
```

## 5. Aprobar la ruta desde el panel de administración de Tailscale

1. Entra a **login.tailscale.com/admin/machines**
2. Busca tu servidor en la lista
3. Click en los tres puntos → **Edit route settings**
4. Habilita (aprueba) el subnet `192.168.100.0/24`

Sin este paso, el servidor anuncia la ruta pero Tailscale no la habilita por seguridad hasta que la apruebes manualmente.

## 6. Instalar Tailscale en tu laptop (cliente)

**Windows/Mac/Linux:** descarga desde tailscale.com/download, o en Linux:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Autentícate con la **misma cuenta** que usaste en el servidor (paso 2).

## 7. Conectar y probar

Con Tailscale activo en tu laptop, ya puedes alcanzar directamente las IPs de la red del instituto, sin VPN manual, sin abrir puertos:

```bash
ssh admin1@192.168.100.200
```

Moodle, desde el navegador:

```
http://192.168.100.200
```

## 8. (Opcional) Nombre en vez de IP

Tailscale asigna automáticamente un nombre tipo MagicDNS a cada dispositivo:

```
servidor-nombre.tailXXXX.ts.net
```

Puedes activarlo en el panel admin → **DNS** → **Enable MagicDNS**, y así conectarte por nombre en vez de memorizar la IP.

## 9. Instalar también en tu celular (opcional, mismo flujo)

App disponible en Play Store / App Store — mismo login, mismo acceso instantáneo a la red del instituto.

---

## Notas

- Gratis hasta 3 usuarios / 100 dispositivos en el plan personal — suficiente para el caso.
- Revoca acceso de un dispositivo en cualquier momento desde el panel admin sin tocar el servidor.
- Verificar el estado de la conexión con:

```bash
tailscale status
```
