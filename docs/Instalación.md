# Guia de instalacion del proyecto Servidor-Moodle-NeuroCode

> **Nota de alcance del proyecto:** esta guía de instalación de Moodle funciona únicamente dentro de la red local del instituto, como repositorio de material y gestión interna.
> No cuenta con dominio público de acceso desde internet. Todas las máquinas que necesiten acceso deben estar conectadas a la red del instituto.

## 1. Instalación y configuración de Ubuntu Server

Con el servidor preparado y conectado con teclado/monitor, se comienza una instalación limpia
de Ubuntu Server. En este caso la versión 26.4 LTS instalada mediante una USB booteable.

A continuación se realiza la configuración de instalación, haciendo enfásis en los siguientes
apartados para que la implementación se lleve acabo con éxito.

### Configuración de red

Debe ser IP estática (no DHCP) para que la IP de acceso no cambie nunca.

| Apartado | Explicación                                                                                                | Ejemplo          |
| -------- | ---------------------------------------------------------------------------------------------------------- | ---------------- |
| Subnet   | El Default Gateway de la red pero se cambia el final por 0/24                                              | 192.168.100.0/24 |
| Address  | La dirección del servidor, se agrega al final un número alto para evitar conflictos con otros dispositivos | 192.168.100.200  |
| Gateway  | El punto de entrada del router (Default Gateway)                                                           | 192.168.100.1    |
| Name     | El DNS propio de la red o bien DNS públicos como los de Google                                             | 8.8.8.8,1.1.1.1  |
| Domain   | El dominio público (no aplica en este caso)                                                                | 'Dejar vacio'    |

_Toda esta información se puede averiguar con el comando **ipconfig** en cualquier equipo conectado a la red._

### Configuración de servidor

Se coloca la información necesaria para el servidor

| Apartado            | Explicación                        |
| ------------------- | ---------------------------------- |
| Nombre              | Nombre del técnico o administrador |
| Nombre del servidor | Algo identificable                 |
| Usuario             | Administrador                      |
| Contraseña          | 'Contraseña segura'                |

### Otros ajustes

1. Marcar OpenSSH, permitirá conectarse remotamente después
2. Omitir "snaps" adicionales que ofrece el instalador, no son necesarios.

## 2. Actualización Inicial

Apartir de ahora podemos continuar manipulando el servidor mediante teclado/monitor o acceder remotamente desde otro dispositivo.

### Conectar al servidor con SSH

```bash
ssh tu_usuario@192.168.100.200
```

```bash
# Configurar autenticación por llave SSH.
ssh-keygen -t ed25519 -C "admin1"
ssh-copy-id admin1@192.168.100.200

# Actualizar el sistema
sudo apt update && sudo apt upgrade -y

# Instalar utilidades básicas
sudo apt install -y curl wget vim htop unzip software-properties-common

# Configurar el firewall (UFW)
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status

# Configurar acceso SSH seguro
sudo nano /etc/ssh/sshd_config
	PermitRootLogin no
	PasswordAuthentication no

# Reiniciar el servicio
sudo systemctl restart ssh

# Configurar zona horaria y hostname
sudo timedatectl set-timezone America/Costa_Rica
sudo hostnamectl set-hostname neurocode_server1

# Fail2ban para protección contra fuerza bruta
sudo apt install -y fail2ban
sudo systemctl enable fail2ban --now
```

## 3. Instalar servidor web Apache

```bash
# Instalar Apache
sudo apt install -y apache2
sudo ufw allow 'Apache Full'   # abre puertos 80 y 443
sudo systemctl enable apache2 --now
```

Verificar entrando a `http://192.168.100.200`

## 4. Instalar PHP y extensiones necesarias para Moodle

Moodle 5.2.1 requiere PHP 8.3 y MySQL 8.4

```bash
# Instalar dependencia para repositorio que contiene PHP 8.3
sudo apt install -y apt-transport-https lsb-release ca-certificates curl gnupg

sudo curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg

echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list

# Instalar PHP 8.3
sudo apt install -y php8.3 php8.3-cli php8.3-common php8.3-mysql php8.3-gd php8.3-xml php8.3-mbstring php8.3-curl php8.3-zip php8.3-intl php8.3-soap php8.3-opcache libapache2-mod-php8.3

# Verificar
php -v

# Ajustar configuracion
sudo nano /etc/php/8.3/apache2/php.ini
	memory_limit = 512M
	max_execution_time = 300
	max_input_time = 300
	max_input_vars = 5000

sudo systemctl restart apache2
```

## 5. Instalar y configurar MySQL

```bash
# Instalar MySQL 8.4
sudo apt install -y mysql-server
sudo mysql_secure_installation
```

_Asistente: contraseña root, eliminar usuarios anónimos, deshabilita login root remoto._

```bash
# Crear base de datos y usuario para Moodle:
sudo mysql -u root -p
```

```sql
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'CONTRASEÑA_SEGURA_AQUI';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

```bash
# Habilita el requisito de Moodle en MySQL:
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Agregar:

```ini
[mysqld]
# Soporte de caracteres para Moodle (utf8mb4)
character-set-server = utf8mb4
collation-server     = utf8mb4_unicode_ci

# Configuración recomendada para Moodle
default_storage_engine = InnoDB
innodb_file_per_table  = 1
```

```bash
# Verificar y reiniciar
sudo mysqld --validate-config
sudo systemctl restart mysql
```

## 6. Descargar e instalar Moodle

```bash
# Descargar rama estable de Moodle
cd /var/www/html
sudo apt install -y git
git clone -b MOODLE_502_STABLE https://github.com/moodle/moodle.git moodle

# Instalacion
sudo mkdir /var/moodledata
sudo chown -R www-data:www-data /var/www/html/moodle
sudo chown -R www-data:www-data /var/moodledata
sudo chmod -R 755 /var/www/html/moodle
sudo chmod -R 777 /var/moodledata
```

## 7. Configurar el Host de Apache (acceso local)

Como el proyecto es de acceso solo interno, el Virtual Host no apunta a un dominio público, sino a un nombre local que se resuelve dentro de la red del instituto.

```bash
sudo nano /etc/apache2/sites-available/moodle.conf
```

```apache
<VirtualHost *:80>
        ServerName 192.168.100.200
        ServerAlias campus.instituto.local
        DocumentRoot /var/www/html/moodle/public

        <Directory /var/www/html/moodle/public>
                Options FollowSymLinks
                AllowOverride All
                Require all granted
                DirectoryIndex index.php
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/moodle_error.log
        CustomLog ${APACHE_LOG_DIR}/moodle_access.log combined
</VirtualHost>
```

```bash
# Activacion
sudo a2enmod rewrite
sudo a2dissite 000-default.conf
sudo a2ensite moodle.conf
sudo systemctl restart apache2
```

## 8. Acceso desde las máquinas del instituto

Dos opciones viables, una más simple, otra más lógica.

### Opción A — Acceso directo por IP

Cualquier equipo conectado a la red puede entrar directamente en:

```
http://192.168.100.200
```

Es la opción más simple, pero poco práctica de recordar/compartir.

### Opción B — DNS local

Algunos routers cuentan con la posibilidad de establecer un DNS local, pero en caso contrario se puede optar por la herramienta dnsmasq.

#### Instalación de dnsmasq

```bash
# Instalar
sudo apt install -y dnsmasq
```

```bash
# Configurar
sudo nano /etc/dnsmasq.conf
```

Agregar línea:

```
address=/campus.instituto.local/192.168.100.200
```

```bash
# Reiniciar
sudo systemctl restart dnsmasq
```

Se debe configurar en el router local que el DNS Server de DHCP automático a que apunte a la IP del servidor. De este forma todas las máquinas accederan automáticamente al servidor como DNS con el nombre de la IP local.

## 9. HTTPS en red local

- **Certificado SSL autofirmado** — se genera localmente sin costo, pero el navegador mostrará una advertencia de "sitio no seguro" que cada usuario debe aceptar una vez por dispositivo.

## 10. Finalizar instalación vía navegador

Entrar a `http://192.168.100.200` o `http://campus.instituto.local` si ya se configuró el acceso por nombre:

- Confirmar rutas de datos
- Conectar con la base de datos (usuario/contraseña que se creó en la sección 5)
- Revisar requisitos
- Crear el usuario Administrador
- Configurar nombre del sitio

## 11. Configurar tareas Cron

```bash
sudo crontab -u www-data -e
```

Agregar:

```
* * * * * /usr/bin/php /var/www/html/moodle/admin/cli/cron.php >/dev/null 2>&1
```

## 12. Configuración general de Moodle

Completar la configuración necesaria para el funcionamiento adecuado de la plataforma, esto en rasgos generales.
Los ajustes específicos que se decidieron aplicar para el Instituo se encuentran en docs/configuracion_especifica.md

**Administración del sitio → General → Configuración del sitio**

- Nombre completo del sitio: ej. "Campus Virtual - Nombre del Instituto"
- Nombre corto: ej. "Instituto"
- Página de inicio: define si muestra login directo o lista de cursos
- Idioma: Español (verifica que el paquete de idioma esté instalado en Administración → Idioma → Paquetes de idioma)

**Apariencia → Tema**

- Por el momento se usa "Boost", el predeterminado que funciona bien (más adelante se desarrolla un tema personalizado).
- Se agrega el logo institucional y color de marca como acento.

### Configurar métodos de autenticación

**Administración del sitio → Extensiones (Plugins) → Autenticación**

Las cuentas se gestionarán de forma manual o por validación de correo.
Al matricular a un estudiante, se crea su usuario directamente en Moodle con la información proporcionada.

**Opciones para creación de Usuario:**

1. Crear usuarios uno a uno al momento de matricular.
2. Crear forma masiva vía CSV **(Administración → Usuarios → Subir usuarios)** si se matricula un grupo completo a la vez.

Formato del CSV para carga masiva:

```csv
username,password,firstname,lastname,email,auth
jperez,Temporal#123,Juan,Pérez,jperez@gmail.com,manual
```

## Crear la estructura de categorías de cursos

**Administración del sitio → Cursos → Gestionar cursos y categorías**

1. Los curso se agrupan por Categorías y Subcategorías.
2. Al crear el curso de define si la duración y secciones del curso.
3. El curso se puede dividir en grupos separados o unificados.
4. A los profesores se les puede asignar uno o varios grupos del mismo curso.

## Configurar roles y permisos

**Administración del sitio → Usuarios → Permisos → Definir roles**

Estructura de Roles (Ya vienen predefinidos, solo asignar):

- **Administrador** → Control completo de la plataforma.
- **Profesor** → Pueden crear/editar contenido y calificar en sus cursos/grupos asignados.
- **Profesor no editor** → Puede gestionar el cotenido y calificar en sus cursos/grupos asignados. NO PUEDE MODIFICAR O CREAR.
- **Estudiante** → Usuario matriculado en un curso - Puede acceder e interactuar según las restricciones del profesor.
- **Usuario** → Cualquier entidad capaz de ingresar a la plataforma.

**Asignación de roles a nivel de curso: dentro de cada curso → Participantes → Inscribir usuarios.**

_Recomendación:_ usar "Cohortes" (Administración → Usuarios → Cohortes) para agrupar estudiantes por curso/grupo y facilitar la inscripción masiva a varios cursos a la vez.

## Repositorio de materiales

- Dentro de cada curso, usar la actividad/recurso "Carpeta" o "Archivo" para subir materiales.
- Para un repositorio institucional centralizado: **Administración del sitio → Extensiones → Repositorios → habilitar "Sistema de archivos del servidor".**

## Calendario, avisos y recordatorios

- **Calendario:** Administración del sitio → General → Calendario
- **Avisos:** cada curso tiene un foro "Novedades" (Announcements) automático
- **Recordatorios:** Administración del sitio → Notificaciones → Mensajería

Verificar que el envío de correo esté configurado correctamente:
**Administración del sitio → Servidor → Correo saliente (SMTP)** — si no se configura, las notificaciones no llegarán por correo a profesores/estudiantes.

## Respaldos automáticos

**Administración del sitio → General → Copias de seguridad → Configuración de copias automáticas**

- Habilitar copias de seguridad automáticas
- Frecuencia recomendada: diaria o cada 2-3 días según actividad
- Definir cuántas copias conservar (ej. últimas 5)
- Ruta de guardado: idealmente fuera de `/var/www/html/moodle`, para poder respaldarla externamente también (ej. disco externo o USB dedicado, dado que no hay nube configurada en este proyecto)

## Seguridad final

**Administración del sitio → Seguridad → Políticas de seguridad del sitio**

- Definir si se fuerza HTTPS (solo aplicable si se configuró el certificado autofirmado de la sección 9) o se mantiene HTTP simple dentro de la red interna.
- Configurar política de contraseñas (longitud mínima, complejidad) o por dejarla por defecto.
- Revisar "Notificaciones de seguridad" para que Moodle avise si detecta configuraciones inseguras.

# Prueba piloto antes del lanzamiento.

1. Crear 1-2 cursos de prueba con contenido real.
2. Invitar a 2 profesores a probar la plataforma.
3. Verificar desde varias máquinas de la red: login, subida de archivos, foros, notificaciones por correo.
4. Corregir lo que falle antes del lanzamiento general.
