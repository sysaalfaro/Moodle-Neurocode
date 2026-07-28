# Configuración Específica de Moodle

## 1. Configuración general del sitio

**Administración del sitio → General → Configuración del sitio**

- Nombre completo del sitio: ej. "Campus Virtual - Instituto NeuroCode"
- Nombre corto: ej. "CV-NeuroCode"
- Página de inicio: define si muestra login directo o lista de cursos
- Idioma: Español (verifica que el paquete de idioma esté instalado en Administración → Idioma → Paquetes de idioma)

**Apariencia → Tema**

- Instala un tema acorde al instituto, o usa "Boost" (el predeterminado, funciona bien)
- Sube el logo institucional en Apariencia → Configuración del tema

## 12.2 Configurar métodos de autenticación

**Administración del sitio → Extensiones (Plugins) → Autenticación**

Las cuentas se gestionarán de forma manual: al matricular a un estudiante, se crea su usuario directamente en Moodle.

**Opción A — Cuentas manuales:**

- Habilita solo "Cuentas manuales"
- Crea usuarios uno a uno al momento de matricular, o de forma masiva vía CSV (Administración → Usuarios → Subir usuarios) si se matricula un grupo completo a la vez

Formato del CSV para carga masiva:

```csv
username,password,firstname,lastname,email,auth
jperez,Temporal123!,Juan,Pérez,jperez@instituto.edu,manual
```

## 12.3 Crear la estructura de categorías de cursos

**Administración del sitio → Cursos → Gestionar cursos y categorías**

## 12.4 Configurar roles y permisos

**Administración del sitio → Usuarios → Permisos → Definir roles**

Roles clave (ya vienen predefinidos, solo asigna):

- **Administrador** → 2 administradores
- **Profesor** → 20-30 profesores (pueden crear/editar contenido y calificar)
- **Estudiante** → ~60 estudiantes

Asignación de roles a nivel de curso: dentro de cada curso → Participantes → Inscribir usuarios.

**Recomendación:** usar "Cohortes" (Administración → Usuarios → Cohortes) para agrupar estudiantes por año/sección y facilitar la inscripción masiva a varios cursos a la vez.

## 12.5 Repositorio de recursos didácticos

- Dentro de cada curso, usar la actividad/recurso "Carpeta" o "Archivo" para subir materiales.
- Para un repositorio institucional centralizado: Administración del sitio → Extensiones → Repositorios → habilitar "Sistema de archivos del servidor" para un repositorio compartido entre cursos.

## 12.6 Calendario, avisos y recordatorios

- **Calendario:** Administración del sitio → General → Calendario
- **Avisos:** cada curso tiene un foro "Novedades" (Announcements) automático
- **Recordatorios:** Administración del sitio → Notificaciones → Mensajería

Verificar que el envío de correo esté configurado correctamente:
**Administración del sitio → Servidor → Correo saliente (SMTP)** — si no se configura, las notificaciones no llegarán por correo a profesores/estudiantes.

## 12.8 Respaldos automáticos

**Administración del sitio → General → Copias de seguridad → Configuración de copias automáticas**

- Habilitar copias de seguridad automáticas
- Frecuencia recomendada: diaria o cada 2-3 días según actividad
- Definir cuántas copias conservar (ej. últimas 5)
- Ruta de guardado: idealmente fuera de `/var/www/html/moodle`, para poder respaldarla externamente también (ej. disco externo o USB dedicado, dado que no hay nube configurada en este proyecto)

## 12.9 Seguridad final

**Administración del sitio → Seguridad → Políticas de seguridad del sitio**

- Definir si se fuerza HTTPS (solo aplicable si se configuró el certificado autofirmado de la sección 9) o se mantiene HTTP simple dentro de la red interna
- Configurar política de contraseñas (longitud mínima, complejidad)
- Revisar "Notificaciones de seguridad" — Moodle avisa si detecta configuraciones inseguras

## 12.10 Prueba piloto antes del lanzamiento

1. Crear 1-2 cursos de prueba con contenido real
2. Invitar a 3-4 profesores "early adopters" a probar antes que el resto
3. Verificar desde varias máquinas de la red: login, subida de archivos, foros, notificaciones por correo
4. Corregir lo que falle antes del lanzamiento general
