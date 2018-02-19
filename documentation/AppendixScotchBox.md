# <a name="part0"></a>Приложение: Scotch Box
Ниже приведено описание того, как установить превосходный [Scotch Box](https://box.scotch.io/) на свой компьютер, чтобы иметь полностью работоспособную среду разработки для XenForo всего за несколько минут с помощью нескольких простых команд.

XenForo имеет настраиваемую конфигурацию Scotch Box, которая предоставляет все необходимое для запуска XenForo, включая отладчик и кеш данных с улучшением производительности.

Scotch Box работает в среде [VirtualBox](https://www.virtualbox.org/)/[Vagrant](https://www.vagrantup.com/).

## <a name="part1"></a>Установка Scotch Box
Начните с определения, где на вашем компьютере вы хотите, чтобы ваш виртуальный веб-сервер сохранял свои файлы. Рекомендуется выбрать место в домашнем каталоге вашего собственного пользователя.

В следующих примерах мы будем использовать каталог под названием *MyServer*, расположенный в корне вашей собственной пользовательской директории, идентифицированный вашим именем *{username}*:

* `/Users/{username}/MyServer` (Mac)
* `C:\Users\{username}\MyServer` (Windows)
* `/home/{username}/MyServer` (некоторые дистрибутивы Linux)
* `/users/{username}/MyServer` (другие дистрибутивы Linux)

После того, как вы выбрали местоположение, выполните следующие действия:

1. Установить [VirtualBox](https://www.virtualbox.org/) на свой компьютер
2. Установить [Vagrant](https://www.vagrantup.com/) на свой компьютер
3. Использовать git клиент, clone `https://github.com/scotch-io/scotch-box` в ваш каталог MyServer. Используя клиент командной строки с приведенным выше примером Mac, команда будет:

   `git clone https://github.com/scotch-io/scotch-box /Users/{username}/MyServer`

4. Once the clone process is complete, download this custom Vagrantfile and overwrite the Vagrantfile that has been created at */Users/{username}/MyServer/Vagrantfile: [Download custom Vagrantfile](https://xenforo.com/xf2-docs/dev/files/scotchbox/Vagrantfile).

5. When the custom Vagrantfile is in place, run the following commands:

   `cd /Users/{username}/MyServer`
   `vagrant up`

Your Scotch Box virtual machine is now created and ready to use.

> **Предупреждение**
> Scotch Box also provides a '[Scotch Box Pro](https://box.scotch.io/pro/)' version of their virtual machine for a reasonable purchase price. If you prefer to run Scotch Box Pro, refer to the [section below describing the differences between configuring and running Scotch Box and Scotch Box Pro](/documentation/AppendixScotchBox.md#part5).

## <a name="part2"></a>Куда идут файлы?
Once your Scotch Box is up and running, you can keep your XenForo PHP and JS files on your host machine, allowing you to use your text editor or IDE of choice, while the virtual machine is responsible for compiling and serving those files through its web server.

You will be able to visit your new web server in your web browser at the following address:

`http://192.168.33.10`

The web server will pull the files to be served from

`/Users/{username}/MyServer/public`

If you want your XenForo to be installed at `http://192.168.33.10/xenforo`, you should place the contents of the upload folder from the XenForo package into `/Users/{username}/MyServer/public/xenforo`.

## <a name="part3"></a>Остановка и перезапуск сервера
You can stop the Scotch Box server at any time by running

    cd /Users/{username}/MyServer
    vagrant halt
... and you can restart it by running

    cd /Users/{username}/MyServer
    vagrant up

> **Предупреждение**
> Although Vagrant / Scotch Box will automatically shut down when you reboot your computer, it will not automatically start up again.
> 
> Whenever you reboot, you will need to run the `vagrant up` command again in order to use the server.

## <a name="part4"></a>Официальная документация
This guide is derived from the official Scotch Box documentation, which is located at [https://box.scotch.io](https://box.scotch.io/)

## <a name="part5"></a>Scotch Box Профессионал
While the basic Scotch Box requires some additional configuration (which is handed through the custom Vagrantfile) in order to run XenForo 2, Scotch Box Pro requires no additional configuration, and is ready to run XenForo 2 without downloading extra packages.

To run [Scotch Box Pro](https://box.scotch.io/pro/), purchase it from the Scotch Box Pro website, then run the git clone command provided as part of the instructions you will receive post-purchase.

You can now install using the same instructions as above, with the single exception that you should download [this custom Vagrantfile](https://xenforo.com/xf2-docs/dev/files/scotchboxpro/Vagrantfile) instead of the one listed in the instructions for Scotch Box.