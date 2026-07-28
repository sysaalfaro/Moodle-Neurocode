# Crear tema hijo de Boost en Moodle

## 1. Estructura base

```bash
cd /var/www/html/moodle/theme
sudo mkdir nombretema
cd nombretema
sudo mkdir lang lang/es layout pix scss templates
```

## 2. Archivo `config.php` (obligatorio)

```bash
sudo nano /var/www/html/moodle/theme/nombretema/config.php
```

```php
<?php
$THEME->name = 'nombretema';
$THEME->parents = ['boost'];
$THEME->sheets = [];
$THEME->editor_sheets = [];
$THEME->scss = function($theme) {
    return theme_nombretema_get_main_scss_content($theme);
};
$THEME->layouts = [];
$THEME->enable_dock = false;
$THEME->yuicssmodules = [];
$THEME->rendererfactory = 'theme_overridden_renderer_factory';
$THEME->requiredblocks = '';
$THEME->addblockposition = BLOCK_ADDBLOCK_POSITION_FLATNAV;
```

## 3. Archivo `version.php` (obligatorio)

```bash
sudo nano /var/www/html/moodle/theme/nombretema/version.php
```

```php
<?php
$plugin->component = 'theme_nombretema';
$plugin->version   = 2026072300;   // formato YYYYMMDDXX
$plugin->requires  = 2024100700;   // build de tu versión de Moodle
$plugin->maturity  = MATURITY_STABLE;
$plugin->release   = '1.0';
$plugin->dependencies = [
    'theme_boost' => 2024100700,
];
```

## 4. String de idioma (obligatorio, mínimo el nombre del tema)

```bash
sudo nano /var/www/html/moodle/theme/nombretema/lang/es/theme_nombretema.php
```

```php
<?php
$string['pluginname'] = 'Nombre Tema Instituto';
$string['choosereadme'] = 'Tema institucional heredado de Boost.';
```

## 5. SCSS — personalización de estilos

**a) Precomputado (variables antes de compilar Boost):**

```bash
sudo nano /var/www/html/moodle/theme/nombretema/scss/pre.scss
```

```scss
$primary: #0057a3; // color institucional
```

**b) Post (overrides finales, después de Boost):**

```bash
sudo nano /var/www/html/moodle/theme/nombretema/scss/post.scss
```

```scss
.navbar {
  background-color: $primary;
}
```

**c) Archivo principal que amarra ambos:**

```bash
sudo nano /var/www/html/moodle/theme/nombretema/scss/nombretema.scss
```

```scss
// Vacío o con imports adicionales si los necesitas
```

## 6. Función para cargar el SCSS (obligatoria, va en `lib.php`)

```bash
sudo nano /var/www/html/moodle/theme/nombretema/lib.php
```

```php
<?php
defined('MOODLE_INTERNAL') || die();

function theme_nombretema_get_main_scss_content($theme) {
    global $CFG;

    $scss = '';
    $filename = !empty($theme->settings->preset) ? $theme->settings->preset : 'default.scss';

    $fs = get_file_storage();
    $context = context_system::instance();

    if ($filename == 'default.scss') {
        $scss .= file_get_contents($CFG->dirroot . '/theme/boost/scss/preset/default.scss');
    }

    $pre = file_get_contents($CFG->dirroot . '/theme/nombretema/scss/pre.scss');
    $post = file_get_contents($CFG->dirroot . '/theme/nombretema/scss/post.scss');

    return $pre . "\n" . $scss . "\n" . $post;
}
```

## 7. Permisos

```bash
sudo chown -R www-data:www-data /var/www/html/moodle/theme/nombretema
sudo chmod -R 755 /var/www/html/moodle/theme/nombretema
```

## 8. Activar el tema

1. Administración del sitio → Apariencia → Temas → Selector de temas
2. Seleccionar `Nombre Tema Instituto`
3. Ir a Administración del sitio → Notificaciones (ejecuta el proceso de instalación del plugin)

## 9. Purgar cachés tras cada cambio de SCSS/PHP

```bash
sudo -u www-data php /var/www/html/moodle/admin/cli/purge_caches.php
```

O vía interfaz: Administración del sitio → Desarrollo → Purgar cachés.

## 10. Logo institucional (opcional, vía admin, no código)

Apariencia → Configuración del tema (una vez activo `nombretema`) → subir logo y favicon.

---

## Notas

- Reemplaza `nombretema` por el nombre real (minúsculas, sin espacios/guiones, ej. `neurocode`)
- Cada cambio en `pre.scss`/`post.scss` requiere purgar caché para verse reflejado
- Para layouts personalizados (ej. header distinto), copiar el archivo `.php` correspondiente desde `theme/boost/layout/` a `theme/nombretema/layout/` y modificarlo ahí
