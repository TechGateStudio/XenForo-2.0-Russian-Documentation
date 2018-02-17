

# Начало работы

Эта документация предназначена для ознакомления вас с разработкой под XenForo 2.0. Предполагается, что перед началом работы с этой документацией вы уже знакомы с базовыми вещами, вроде PHP и MySQL. Опыт работы с предыдущими версиями XenForo не требуется, но предоставит ряд преимуществ.

На последующих страницах документации мы расскажем об установке локального сервера, подготовке к установке и самом процессе чистой установки XenForo 2.0 и объясним некоторые базовые концепции разработки под XF2.

## Что нового для разработчиков?

Хотя XenForo 2.0 добавляет много улучшений для ваших форумов и его участников, значительные усилия были направлены на улучшение базовой структуры XenForo. Вы можете прочитать дополнительную информацию об этих изменениях в следующих темах:

[Что нового для разработчиков в XenForo 2 (часть 1)](https://xf2demo.xenforo.com/threads/whats-new-for-developers-in-xenforo-2-part-1.1297/)<sup><a href="#Prim1">1</a></sup>

[Что нового для разработчиков в XenForo 2 (часть 2)](https://xf2demo.xenforo.com/threads/whats-new-for-developers-in-xenforo-2-part-2.1409/)<sup><a href="#Prim1">1</a></sup>
## Начало работы
Начать разработку на XF легко. Вам просто нужно скачать файлы, загрузить их на веб-сервер и запустить установку.

Если у вас еще нет веб-сервера, не беспокойтесь, вы легко сможете настроить его в локальном окружении.

## Загрузка XF 2.0
To download XF 2.0, just visit the [Customer Area](https://xenforo.com/customers) and log in as normal. Locate the correct license and click the "Download XenForo" link. Select the version you wish to download, the package type and accept the license agreement. Finally, click the Download button to download the files.

## Системные требования XF 2.0
Требования к запуску XF 2.0 изменились с XF 1.5. Рекомендуемые требования:

* PHP: 5.4.0+
* MySQL: 5.5+
* PHP расширения: MySQLi, GD (with JPEG support), PCRE, SPL, SimpleXML, DOM, JSON, iconv, ctype, cURL

[Загрузить скрипт проверки требований.](https://xenforo.com/xf2-docs/dev/files/xenforo2-requirements-test.zip)

## Настройка локального сервера
It's often more convenient to set up a local web server for development. There are generally two approaches for this:

Install Apache (or nginx), MySQL (or MariaDB) and PHP yourself.
Install a pre-built virtual machine
Install a pre-built stack.
Setting things up yourself is more complicated, but tends to give you more control over how everything is set up.

### Предварительно построенная виртуальная машина
There are a variety of pre-built virtual machines available on the Internet, which provide the advantage of having all of the necessary services to run XenForo neatly packaged into one place, without having to install and maintain them directly on your own computer.

Some of the XenForo developers use a virtual machine called [Scotch Box](https://box.scotch.io/), which includes everything you need to run XenForo with zero configuration required. We have a [step-by-step guide](https://xenforo.com/xf2-docs/dev/scotchbox/) to getting a XenForo development server up and running - you can have a working virtual web and database server up and running in just a few minutes by running a handful of commands.

[Installing Scotch Box for use with XenForo](https://xenforo.com/xf2-docs/dev/scotchbox/)

### Готовые сборки
There are many pre-built stacks out there and they may vary in feature set, performance and reliability. Bitnami maintain a number of stacks, including [LAMP](https://bitnami.com/stack/lamp), [MAMP](https://bitnami.com/stack/mamp) and [WAMP](https://bitnami.com/stack/wamp) stacks for use on Linux, Mac and Windows respectively. They all include a fully configured installation of Apache, MySQL and PHP and include PhpMyAdmin for managing MySQL.

## Загрузка
To install XF 2.0, you simply need to extract the ZIP file downloaded from the Customer Area and upload some of the files and directories within.

Once extracted you will see a directory named `upload`. You need to go into that directory and upload the files and directories to your server's web root. This would usually be in a directory named `public_html`, `htdocs` or `www`.

## Создание src/config.php
If using the CLI to install XF 2.0, you will need to create the config.php file manually. To do this, enter the `src` directory within the XF 2.0 files you uploaded to your server. Create a new file named config.php and populate it with the host, port, username, password and database name for your MySQL server.
> **Note**
> Make sure you create the config file in within the src directory. The library directory is only used for legacy purposes.

Once finished, it should look like the following:

    <?php
    
    $config['db']['host'] = 'localhost';
    $config['db']['port'] = '3306';
    $config['db']['username'] = 'root';
    $config['db']['password'] = 'mypassword';
    $config['db']['dbname'] = 'xf2';

You're now ready to install!

If you are using MySQL 5.5 and above and you wish to have full unicode support (for things like emoji) you should also add the following before install:

    $config['fullUnicode'] = true;

## Замечание по правам доступа к файлам
XenForo will need to write files to specific locations while running. In normal operation, this is limited to the `data` and `internal_data` directories (and their sub-directories). These file writes will be triggered by things like attachment uploads, so they will normally be triggered by the user PHP as running as within your web server. Therefore, it is necessary to ensure that permissions are set in these directories so that the web server can write to them. You will need to do this before installation can begin.

When the CLI is involved, this situation gets trickier as there are now potentially two users that need to be able to write to the files. As such, it's important to take steps to avoid problems writing to these files. Here are a few options.

1. Use the same user for the CLI and the web server. This may take the form of you switching to the web server user before running any installation or upgrade command (or any other that will write files).
If available, consider applying ACLs to the `data` and `internal_data` directories. 2. This concept varies by OS and configuration, but the general idea is described [здесь](http://symfony.com/doc/current/setup/file_permissions.html).
3. Force specific permissions on what is written by PHP. This can be done via the src/config.php file with a line like this: `$config['chmodWritableValue'] = 0666;` This approach is potentially the simplest for development purposes.

Note that if you are developing add-ons, you may potentially have other locations that need to be written to by the CLI and web server users. Notably, this includes the `_output` directory within add-ons. In this situation, having your web server run as your CLI user may cause the least friction. If you go down any other route, you may need to ensure that your web server can write to your entire XenForo installation; this is not recommended in production.

## Установка
The current way to install XF 2.0 is via the new CLI system. A lot of development processes can only be performed using the CLI so let's get stuck into using it to install XF 2.0. To run these commands, you will need access to a terminal/shell, the php CLI command and the current working directory should be the root of where you uploaded the XF 2.0 files.

> **Warning** 
> To eliminate file permission problems, we recommend running the installer as the same user that PHP runs as via your web server. If you don't do this, you should take steps to ensure that permissions are set correctly. See the above section for more details.

To start the install, just enter the below command:

    $ php cmd.php xf:install

You will be asked a number of questions, such as the initial administrator username and password, board title. After this, the XF 2.0 database tables and master data will be imported.

XF 2.0 is now installed!

## Переустановка
Occasionally it may be necessary to reinstall XF2. This is particularly true during the Development Preview stage which does not support upgrading. If you are ready to do a reinstall, download the new files (if applicable) as per the [Загрузка XF 2.0](https://xenforo.com/xf2-docs/dev/#downloading-xf-20) section above. It should generally be possible to just merge and overwrite your existing files. If you're doing a full clean re-install, you may want to save a copy of your config.php file or re-create it as per the instructions in the [Создание src/config.php](https://xenforo.com/xf2-docs/dev/#creating-srcconfigphp).

Before uploading the new files, you should delete the contents of your data and internal_data directories.

Finally, you will just need to start the installation, similar to above. You will need to use the --clear option which will delete all of the existing xf_ tables.

    $ php cmd.php xf:install --clear

Once the re-install has been completed, you should now be able to log back on.

If you have been developing add-ons, and you have chosen to keep or backup your existing `src/addons` directory, you can restore your add-on data with the [Import development output](https://xenforo.com/xf2-docs/dev/development-tools/#import-development-output) command.

> **Warning** 
> Be careful if you choose to back up and restore your `src/addons` directory. The `XF` directory within contains the XF master data, and should not be restored from a backup to ensure you always have the most up to date version of the files.
> 
> Performing a reinstall in this way is a destructive operation and it will remove all data you have created. Additionally, bear in mind that only tables with the `xf_` prefix are cleared. This is a significant reason for the recommendation that all tables, even for add-ons, should be prefixed with `xf_`.

## Провека целостности файлов
When you install XF2, we perform a file integrity check in the installation. If necessary, and you can't otherwise perform the check via the page in the Admin CP, you can run the CLI command to perform that check.

    $ php cmd.php xf:file-check [addon_id]

If you wish to do a file health check on all files, including XF itself, just omit the `[addon_id]` argument. For XF only, just use XF in place of the argument, or for a specific add-on, just specify the add-on ID you wish to check.

## Команды управления плагинами
In addition to the above commands for installing XF2, there are also several commands for managing add-ons.

### Установка
    $ php cmd.php xf:addon-install [addon_id]

Installs the specified add-on, as long as it is available, and passes the file health check. If development output is available, you will be asked to confirm if you wish to use that for the installation, instead of the exported data XML files.

### Обновление
    $ php cmd.php xf:addon-upgrade [addon_id]

Upgrades the specified add-on, as long as it is upgradeable, and passes the file health check. Can optionally perform import from development output.

### Перестроение
    $ php cmd.php xf:addon-rebuild [addon_id]

Rebuilds the master data for the specified add-on, as long as it is rebuildable, and passes the file health check. This re-imports the add-on's data. Can optionally perform import from development output.

### Удаление
    $ php cmd.php xf:addon-uninstall [addon_id]

Uninstalls the specified add-on, as long as it is uninstallable.

# Примечание
1. <a name="Prim1"></a>Не рабочие ссылки так и указаны в документации. Если поправят - сообщите.
