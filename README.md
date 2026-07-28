# Aula Virtual - Instituto Tecnológico Neurocode

El proyecto consiste en la implementación de la plataforma LMS Moodle para el Insituto Tecnológico Neurocode ubicado en
San José.

El alcanze del proyecto es a nivel local dentro de la red institucional, con la finalidad de centralizar la gestión académica,
el repositorio de materiales didácticos y facilitar la modalidad virtual de los cursos.

## Cronograma

| Fase           | Descripción                                                                         | Fecha                               |
| -------------- | ----------------------------------------------------------------------------------- | ----------------------------------- |
| Prueba         | Test de instalación y configuración dentro de un Máquina Virtual                    | 19/07/2026 - 24/07/2026             |
| Preparación    | Establecimiento de configuraciones, parámetros y características necesarias         | 25 de Julio - 26 de Julio del 2026  |
| Implementación | Instalación del sistema operativo, entorno y plataforma en el servidor físico       | 26 de Julio - 01 de Agosto del 2026 |
| Configuración  | Configuración de características, personalización y funcionamiento de la plataforma | Por confirmar...                    |
| Adaptación     | Pruebas piloto en las máquinas con los primeros cursos y usuarios                   | Por confirmar...                    |
| Correción      | Ajustes ante posibles fallos o sugerencias de mejora                                | Por confirmar...                    |
| Documentación  | Recopilación de guías, screnshoots y aprendizajes dentro de un repositorio          | Por confirmar...                    |
| Mantenimiento  | Revisión y cuidado del servidor y la plataforma para su correcto funcionamiento     | Continuo...                         |

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

#### Hardware

     - Por confirmar..

#### Software

     - Ubuntu Server 26.04

### Stack tecnológico

Se optó por el uso de las versiones del stack compatible con la versión más reciente de Moodle en el momento de
la implementación, Modle 5.2.1, acompañado del stack web con el que se construyo la landing page.

##### Moodle LMS

     - Apache: versión
     - PHP: versión (8.3)
     - MySQL: versión (8.4)
     - Moodle: versión (5.2.1+)

### Dominios y DNS

<!-- Por confirmar... -->

## Instalación y despliegue

Ver docs/instalacion.md para el proceso completo de instalación del stack.

## Gestión de usuarios

1. Las cuentas se crean manualmente al matricular al estudiante.
2. Posteriormente se realiza la verificación por correo y el cambio de contraseña.
3. Solo los roles administrativos tienen permiso de crear cuentas.

## Seguridad

### SSL/HTTPS forzado

### Autenticación SSH por llave (si aplica)

### Firewall (UFW) y puertos abiertos

### Política de contraseñas

## Respaldos (Backups)

### Política de respaldos

     - Frecuencia

### Proceso de respaldos

## Mantenimiento

### Actualizaciones del stack

### Actualizaciones del server

## Solución de problemas

## Contactos y responsables

- Administrados y técnico:
- Responsable académico:

## Licencia y créditos

Uso interno - Instituto Tecnológico Neurocode
