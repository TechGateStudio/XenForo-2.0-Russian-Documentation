# <a name="part0"></a>Проектирование стилей
In XF2 we have introduced an all new way to build and edit styles called "Designer mode". Designer mode is a collection of CLI tools that allow you to modify certain templates within a style directly on the file system. It also outputs various metadata and information about style properties which is useful for version control and collaboration.

## <a name="part1"></a>Включение режима дизайнера
The first step to enabling designer mode is to enable it in `config.php`:
```php
$config['designer']['enabled'] = true;
```
Optionally, you can also specify a different path for designer mode files to exist on the file system. The following represents the default location. To change the location, add the below to your `config.php` and modify the path accordingly:
```php
$config['designer']['basePath'] = 'src/styles';
```

## <a name="part2"></a>Включение режима дизайнера для стиля
Designer mode must be explicitly enabled for each style. We enable designer mode on a style by using the CLI and specifying the style ID of the style, and choosing a "designer mode ID":
```bash
php cmd.php xf-designer:enable [style_id] [designer_mode_id]
```
The designer mode ID is the identifier you will use for future commands related to designer mode. Once enabled, the current modified components of the style will be exported to the `[basePath]/[designer_mode_id]` directory.

When enabling designer mode for this style if that directory already exists you will be given a choice to make as to whether we should overwrite the current contents of that directory from the style, or whether we should overwrite the current style from the current contents of that directory.

## <a name="part3"></a>Отключение режима дизайнера для стиля
To disable designer mode for a style, you just run the following CLI command:
```bash
php cmd.php xf-designer:disable [designer_mode_id]
```
By default, this will keep the copy of the designer mode output on the file system. To remove the data, you can run the same command with the `--clear` option:
```bash
php cmd.php xf-designer:disable [designer_mode_id] --clear
```

## <a name="part4"></a>Что выводится и где?
It is important to remember that a style within XF only consists of what is *modified in that style*. This means that designer mode output will only consist of what has been modified in the style. Templates and style properties which are modified in a parent style is not output.

### <a name="part5"></a>Шаблоны
Templates will be output to the `[basePath]/[designer_mode_id]/templates` directory. Within that directory you may have another directory for each type (e.g. admin, email and public).

The templates will be output in HTML format and are directly editable on the file system. Changes made on the file system are imported and compiled when that template is loaded on a page. Similarly, you can revert a template by deleting it from the file system (if it was previously modified).

### <a name="part6"></a>Группы и параметры стилей
Style properties and groups will be output to the `[basePath]/[designer_mode_id]/style_properties` and `[basePath]/[designer_mode_id]/style_property_groups` directories. They are exported in JSON format and serve as a useful way to monitor changes to these files via a version control system.

It is not recommended to modify these files directly as changes to them will *not* be automatically imported like they are with templates.

## <a name="part7"></a>Изменение конкретного шаблона
Bearing in mind that a style represents components which are modified within that style only, when designer mode is enabled, the file system will also contain only components which are modified within that style only. It would not be possible to output the effective version of each template and style property.

To mark a template as modified within a style, you can do it in the usual way by editing it in the Admin CP. Templates and style properties modified in the Admin CP will automatically be written out to the file system if designer mode is enabled. However, it would likely be more convenient to modify or "touch" a template using a CLI command:
```bash
php cmd.php xf-designer:touch_template [designer_mode_id] [template_type:template_title]
```
As long as the specified template exists in a parent or the master style, it will be copied to the current style and output to the file system. You can then modify the template directly in the file system.

If you would like to create an entirely custom template in your style (that doesn't exist in any other style within the tree), you can use the same command but you would just pass the `--custom` option:
```bash
php cmd.php xf-designer:touch_template [designer_mode_id] [template_type:template_title] --custom
```

## <a name="part8"></a>Другие полезные команды
There are a number of other useful commands relating to designer mode:

### <a name="part9"></a>Экспорт в базу данных
This command is usually automatically run when designer mode is enabled on a style, but if for some reason you would like to overwrite the file system copy with what is currently in the database, then you can run the following command:
```bash
php cmd.php xf-designer:export [designer_mode_id]
```
It's also possible to export only specific types, e.g. `xf-designer:export-templates`.

### <a name="part10"></a>Импорт в файловую систему
This command will overwrite the database copy of the style with what is on the file system:
```bash
php cmd.php xf-designer:import [designer_mode_id]
```
It's also possible to import only specific types, e.g. `xf-designer:import-templates`.

### <a name="part11"></a>Синхронизация шаблонов
This command is similar to importing templates (see above) but instead of overwriting everything it will only import templates and recompile them if the metadata has changed. It will also apply version number updates accordingly.
```bash
php cmd.php xf-designer:sync-templates [designer_mode_id]
```

### <a name="part12"></a>Обратный шаблон
This command can be used to revert a template, effectively deleting the custom version from the current style.
```bash
php cmd.php xf-designer:revert-template [designer_mode_id] [template_type:template_title]
```
It's also possible to trigger a revert by removing the template from the file system.