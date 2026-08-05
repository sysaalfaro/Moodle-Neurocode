# Crear tema hijo de Boost en Moodle

## 1. Estructura base

```bash
cd /var/www/html/moodle/theme
sudo mkdir ncthemev1
cd ncthemev1
sudo mkdir lang lang/es layout pix scss templates
```

## 2. Archivo `config.php` (obligatorio)

```bash
sudo nano /var/www/html/moodle/theme/ncthemev1/config.php
```

```php
<?php
$THEME->name = 'ncthemev1';
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
sudo nano /var/www/html/moodle/theme/ncthemev1/version.php
```

```php
<?php
$plugin->component = 'theme_ncthemev1';
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
sudo nano /var/www/html/moodle/theme/ncthemev1/lang/es/theme_ncthemev1.php
```

```php
<?php
$string['pluginname'] = 'Tema Instituto NeuroCode';
$string['choosereadme'] = 'Tema institucional heredado de Boost.';
```

## 5. SCSS — personalización de estilos

**a) Precomputado (variables antes de compilar Boost):**

```bash
sudo nano /var/www/html/moodle/theme/ncthemev1/scss/pre.scss
```

```scss
$primary: #0057a3; // color institucional
```

**b) Post (overrides finales, después de Boost):**

```bash
sudo nano /var/www/html/moodle/theme/ncthemev1/scss/post.scss
```

```scss
.navbar {
  background-color: $primary;
}
```

**c) Archivo principal que amarra ambos:**

```bash
sudo nano /var/www/html/moodle/theme/ncthemev1/scss/ncthemev1.scss
```

```scss
// Vacío o con imports adicionales si los necesitas
```

## 6. Función para cargar el SCSS (obligatoria, va en `lib.php`)

```bash
sudo nano /var/www/html/moodle/theme/ncthemev1/lib.php
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

    $pre = file_get_contents($CFG->dirroot . '/theme/ncthemev1/scss/pre.scss');
    $post = file_get_contents($CFG->dirroot . '/theme/ncthemev1/scss/post.scss');

    return $pre . "\n" . $scss . "\n" . $post;
}
```

## 7. Permisos

```bash
sudo chown -R www-data:www-data /var/www/html/moodle/theme/ncthemev1
sudo chmod -R 755 /var/www/html/moodle/theme/ncthemev1
```

## 8. Activar el tema

1. Administración del sitio → Apariencia → Temas → Selector de temas
2. Seleccionar `Tema Instituto Neurocode`
3. Ir a Administración del sitio → Notificaciones (ejecuta el proceso de instalación del plugin)

## 9. Purgar cachés tras cada cambio de SCSS/PHP

```bash
sudo -u www-data php /var/www/html/moodle/admin/cli/purge_caches.php
```

O vía interfaz: Administración del sitio → Desarrollo → Purgar cachés.

## 10. Logo institucional (opcional, vía admin, no código)

Apariencia → Configuración del tema (una vez activo `ncthemev1`) → subir logo y favicon.

---

## Notas

- Cada cambio en `pre.scss`/`post.scss` requiere purgar caché para verse reflejado
- Para layouts personalizados copiar el archivo `.php` correspondiente desde `theme/boost/layout/` a `theme/ncthemev1a/layout/` y modificarlo ahí
