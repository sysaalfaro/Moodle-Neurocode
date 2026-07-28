# Aula Virtual - Instituto Tecnológico Neurocode

El proyecto consiste en la implementación de la plataforma LMS Moodle para el Insituto Tecnológico Neurocode ubicado en
San José.

El alcanze del proyecto es a nivel local dentro de la red institucional, con la finalidad de centralizar la gestión académica,
el repositorio de materiales didácticos y facilitar la modalidad virtual de los cursos.

## Cronograma

| Fase           | Descripción                                                                         | Fecha                               |
| -------------- | ----------------------------------------------------------------------------------- | ----------------------------------- |
| Prueba         | Test de instalación y configuración dentro de un Máquina Virtual                    | 19/07/2026 - 24/07/2026             |
| Preparación    | Establecimiento de configuraciones, parámetros y características necesarias         | 25-26 de Julio del 2026             |
| Implementación | Instalación del sistema operativo, entorno y plataforma en el servidor físico       | 26 de Julio - 01 de Agosto del 2026 |
| Configuración  | Configuración de características, personalización y funcionamiento de la plataforma | 19-26 de Julio del 2026             |
| Adaptación     | Pruebas piloto en las máquinas con los primeros cursos y usuarios                   | 19-26 de Julio del 2026             |
| Correción      | Ajustes ante posibles fallos o sugerencias de mejora                                | 19-26 de Julio del 2026             |
| Documentación  | Recopilación de guías, screnshoots y aprendizajes dentro de un repositorio          | 19-26 de Julio del 2026             |
| Mantenimiento  | Revisión y cuidado del servidor y la plataforma para su correcto funcionamiento     | Continuo... (Hasta la fecha)        |

## Fase de prueba

Al ser este mi primer proyecto de alcanze profesional, opté por realizar todas las pruebas necesarias y el autoaprendizaje
necesario para llevar a cabo el proyecto mediante una prueba de implementación en una máquina virtual instalada en mi PC.

Con el apoyo de los fundamentos obtenidos durante lo progresado en mi bachillerato de Ingeniería en Sistemas en la Universidad
Tecnológica Costarricense, sumado a recursos de aprendizaje en plataformas como YouTube y la documentación oficial de Moodle.

### Simulación del servidor

Se configuró una máquina virtual con VMware con las siguientes especifícaciones:

1. 4GB de RAM, CPU 2, 30GB de disco.
2. Adaptador de red "Bridge " con el fin de simular la conexión a la misma red entre el servidor real y la pc de configuración.

## Arquitectura del proyecto

Servidor local físico:

- Por confirmar..
  IPs
  nombres de host.

<!-- Diagrama  -->

### Infraestructura

<!-- Tabla o lista con los datos técnicos del servidor:
     - Sistema operativo y versión (Ubuntu Server 26.04)
     - Specs: RAM, almacenamiento, CPU
     - IP interna / IP pública (si aplica)
     - Ubicación física del servidor -->

### Stack tecnológico

Se optó por el uso de las versiones del stack compatible con la versión más reciente de Moodle en el momento de
la implementación, Modle 5.2.1, acompañado del stack web con el que se construyo la landing page.

##### Moodle LMS

     - Apache: versión
     - PHP: versión (8.3)
     - MySQL: versión (8.4)
     - Moodle: versión (5.2.1+)
     - Certbot/Let's Encrypt

#### Landing Page

    - HTML5
    - CSS Tailwind

### 🌐 Dominios y DNS

<!-- Documenta la estructura de dominio/subdominio:
     - dominio.edu → landing
     - campus.dominio.edu → Moodle
     Indica dónde está gestionado el DNS (proveedor), y qué registros
     apuntan a qué (A record, CNAME, etc.) -->

## Instalación y despliegue

<!-- Aquí NO repitas toda la guía paso a paso (eso puede ir en un archivo
     aparte tipo INSTALL.md o docs/). En el README, solo un resumen de
     alto nivel con referencia al documento detallado, por ejemplo:
     "Ver docs/instalacion-servidor.md para el proceso completo de
     instalación del stack (Apache, PHP, MySQL, Moodle)." -->

## Gestión de usuarios

<!-- Documenta el flujo real que van a usar:
     - Cuentas se crean manualmente al matricular
     - Quién tiene permiso de crear cuentas (roles administrativos)
     - Formato de username/correo institucional que usan
     - Referencia a dónde está el procedimiento paso a paso (puede ir
       en un archivo separado docs/gestion-usuarios.md) -->

## Seguridad

<!-- Lista de medidas implementadas, sin dar detalles sensibles como
     contraseñas o IPs exactas si el repo es público:
     - SSL/HTTPS forzado
     - Autenticación SSH por llave (si aplica)
     - Firewall (UFW) y puertos abiertos
     - Política de contraseñas
     - fail2ban (si lo implementaste) -->

## Respaldos (Backups)

<!-- Documenta:
     - Qué se respalda (base de datos, moodledata, config.php)
     - Frecuencia
     - Dónde se guardan las copias (ruta local, servidor externo, nube)
     - Cómo restaurar un backup (o referencia a docs/restauracion.md) -->

## Mantenimiento

<!-- Tareas periódicas que alguien debe hacer:
     - Actualizaciones de Moodle (cuándo y cómo)
     - Actualizaciones de seguridad del sistema (apt upgrade)
     - Renovación de certificado SSL (aunque certbot lo automatiza,
       documenta cómo verificar que sigue funcionando) -->

## Solución de problemas comunes

<!-- Lista corta de problemas que ya resolviste durante el montaje
     y cómo los solucionaste (ej. el tema del plugin auth_socket en
     MySQL, o el error de UFW con Apache Full). Esto ahorra tiempo
     enorme a quien retome el proyecto después, incluyéndote a ti
     mismo en 6 meses. -->

## Contactos y responsables

<!-- Quién administra qué:
     - Administrador técnico del servidor (tú, y tu backup)
     - Responsable académico/pedagógico de Moodle
     - Proveedor de internet / dominio (si aplica) -->

## Licencia y créditos

<!-- Si es un proyecto interno del instituto, puede ser simplemente
     "Uso interno - Instituto XXXX". Si usas algún tema o plugin de
     terceros en Moodle, acredítalos aquí. -->
