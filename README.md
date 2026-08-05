# Aula Virtual - Instituto Tecnológico Neurocode

El proyecto consiste en la implementación de la plataforma LMS Moodle para el Insituto Tecnológico Neurocode ubicado en
San José.

El alcanze del proyecto es a nivel local dentro de la red institucional, con la finalidad de centralizar la gestión académica,
el repositorio de materiales didácticos y facilitar la modalidad virtual de los cursos.

## Cronograma

| Fase           | Descripción                                                                         | Fecha                   |
| -------------- | ----------------------------------------------------------------------------------- | ----------------------- |
| Prueba         | Test de instalación y configuración dentro de un Máquina Virtual                    | 19/07/2026 - 24/07/2026 |
| Preparación    | Establecimiento de configuraciones, parámetros y características necesarias         | 25 de Julio del 2026    |
| Implementación | Instalación del sistema operativo, entorno y plataforma en el servidor físico       | 4 de Agosto del 2026    |
| Configuración  | Configuración de características, personalización y funcionamiento de la plataforma | 6 de Agosto del 2026    |
| Adaptación     | Pruebas piloto en las máquinas con los primeros cursos y usuarios                   | Por confirmar...        |
| Correción      | Ajustes ante posibles fallos o sugerencias de mejora                                | Por confirmar...        |
| Mantenimiento  | Revisión y cuidado del servidor y la plataforma para su correcto funcionamiento     | Continuo...             |

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

Servidor Físico:

- IP: 192.168.100.200
- Host: neurocode-server1

<!-- Diagrama  -->

### Infraestructura

Hardware:

- Case = Dell Dimension E520
- CPU =
- RAM = 10GB DDR4
- Disco = 2TB HDD

Software:

- Ubuntu Server 26.04
- Ubuntu Desktop 24.04 LTS

### Stack tecnológico

Moodle utilizar el stack denominado 'LAMP'

![LAMP Stack](/images/Stack.jpg)

Se optó por el uso de las versiones del stack compatible con la versión más reciente de Moodle en el momento de
la implementación, Modle 5.2.1.

##### Moodle LMS

     - Apache: versión (2.4)
     - PHP: versión (8.3)
     - MySQL: versión (8.4)
     - Moodle: versión (5.2.1+)

### Dominios y DNS

<!-- No aplica por el momento... -->

## Instalación y despliegue

Ver [Instalacion.md](/docs/Instalacion.md) para el proceso completo de instalación del stack.

## Gestión de usuarios

1. Las cuentas se crean manualmente al matricular al estudiante con su identificación.
2. Posteriormente se realiza la verificación por correo y el cambio de contraseña.
3. Solo los roles administrativos tienen permiso de crear cuentas.

## Seguridad

Las políticas se seguridad son mínimas en esta fase ya que el servidor no está expuesto a la red pública.

### SSL/HTTPS

<!-- No aplica de momento... -->

### Autenticación SSH por llave

Para lograr acceder desde otro dispositivo al servidor, se utiliza OpenSSH, este protocolo permite controlar el equipo desde una terminal externa. Ahora para que esto sea seguro la primera vez debemos acceder con la contraseña del servidor e inmediatamente generar una Key en nuestro dispositivo que el servidor identifique ante cualquier intento de ingreso posterior, evitando accesos no consentidos mediante otros métodos.

Se detalla el procedimiento en: [Instalacion.md](/docs/Instalacion.md).

### Firewall (UFW) y puertos abiertos

Se configuro el firewall de manera que el acceso de los puertos quede límitado a los sistemas necesarios:

- Puerto 80 y 433 para la comunicación con el servidor Apache
- Puerto para la conexión SSH

### Política de contraseñas

Por el momento las contraseñas son simplificadas, el usuario inicia sesión con su identificación.

## Respaldos (Backups)

<!-- Por confirmar... -->

### Política de respaldos

### Proceso de respaldos

## Mantenimiento

<!-- Por confirmar... -->

### Actualizaciones del stack

### Actualizaciones del server

## Solución de problemas

### Ubuntu Server vs Ubuntu Desktop

Las fase de pruebas en máquina virtual se ejecutó utilizando Ubuntu Server como SO, en la fase de instalación real el Instituto decidió utilizar Ubuntu Desktop en su lugar, esto da lugar a unas mínimas discrepancias con la instalación del stack a tener en cuenta.

1. Configuración de la red:
   Al realizar el proceso de instalación del SO, Ubuntu Desktop no pide ajustar los parámetros de red necesarios para correcto funcionamiento del servidor, pues no está pensado para ello. Por ende al finalizar el proceso se debe acceder a Configuración → Red → Cableado, y ajustar la IP de forma Manual con los parámetros indicados al inicio de [Instalacion.md](/docs/Instalacion.md).

2. Falta de programas y dependencias:
   Ubuntu Desktop por defecto no trae OpenSSH y cron, ambos necesarios para el cumplimiento del proceso de instalación. Esto se soluciona con dos comandos simples en terminal:

   ```bash
        sudoapt install -y OpenSSH
   ```

   ```bash
        sudoapt install -y cron
   ```

3. Posibles problemas de rendimiento:
   El utilizar un entorno gráfico completo como el de Ubuntu Desktop puede simplificar las cosas especialmente para "no técnicos" a la hora de administrar el servidor, sin embargo debe tener en cuenta que esto produce una caída en el rendimiento y la eficiencia de las peticiones del servidor y la web de 'Moodle', que puede ser más o menos severa dependiendo de los recursos de hardware disponibles.

_Mi sugerencia es utilizar Ubuntu Server en estos casos, para obtener un rendimiento óptimo y evitar otros conflictos_

### IP DHCP vs IP Manual (Estática)

Este apartado es de suma importancia para el proyecto, las redes por defecto comúnmente cuentan con sistemas de IP dinámicas o DHCP que constantemente cambian la IP cada cierto tiempo, esto favorece significativamente la seguridad de la red. Esto aplica tanto para el router como para nuestro servidor.

Por el momento no es necesario configurar una IP estática en el router porque aún la página no será pública, el servidor inicializado por nosotros se conecta a la red por cable y le es **inherente ese cambio de IP**. Sin embargo, nuestro servidor que será el encargado de enrutar la dirección del servidor apache dónde se alojará nuestro Moodle si debe contar **obligatoriamente con una IP estática configurada manualmente** como se vió en la guía.

De lo contrario la IP puede cambiar de dirección en cualquier momento, lo que romperá nuestras configuraciones y el funcionamiento de la página web Moodle.

### OpenSSH Key

1. Conflicto menor con la Key:
   En mi equipo personal desde el cual me conecto al servidor, la llave ya estaba generada debido a las pruebas que realizad en la Máquina Virtual. Esto no debería generar inconvenientes ya que una sola llave sirve para conectarse a cualquier servidor, este último solo la copia y la guarda como un método de autenticación. El único detalle es que tanto el servidor virtual como el físico se configuraron con la misma IP, lo que dió lugar a un promt de advertencia algo intimidante pero que se soluciona simplemente regenerando la llave con:

   ```bash
   ssh-keygen -f '/home/user/.ssh/known_hosts' -R '192.168.100.200' #cambiar 'user' por tu usuario.
   ```

   Y luego se reintenta la conexión.

## Contactos y responsables

- Administrador y técnico:
  Anthony Gabriel Alfaro Mejía
  sysaalfaro@gmail.com

- Responsable académico:
  José Rodolfo Herrera Baca

## Licencia y créditos

Uso interno - Instituto Tecnológico Neurocode
