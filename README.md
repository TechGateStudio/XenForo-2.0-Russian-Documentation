# XenForo-2.0-Russian-Documentation
Русский перевод документации XenForo 2.0

**Переводчик:** Нет тут ничего такого занятного, мне просто захотелось перевести документацию XF2. Я уже более 3 месяцев использую XenForo 2.0 и за это время уже достаточно в ней разобрался и свыкся с её работой, но все же остаются темные моменты или моменты которые к сожалнию не запомнились, в следствие чего приходится обращатся за помощью к документации, каждый раз переводя тот или иной участок текста заново. Мне это немного надоело я решил выполнить перевод и выложить его в свободный доступ. Как говорится, "просто захотелось", но просто не бывает :). 

**Приветствуются правки ошибок и описаний, а так же дополнение информации.**

----------------

**Состояние перевода:**
* Общее состояние: [##########>---------------------------------------] 21.4%
* Начало работы: [##################################################>] 99%
* Структура плагина: [>--------------------------------------------------] 1%
* Инструменты разработки: [###############>----------------------------------] 30%
* Общие понятия: [>--------------------------------------------------] 1%
* Основы маршрутизации: [>--------------------------------------------------] 1%
* Основы контроллера: [>--------------------------------------------------] 1%
* Сущности, Поисковики и Репозитории: [>--------------------------------------------------] 1%
* Управление схемой: [##################################################>] 99%
* Давайте построим плагин: [>--------------------------------------------------] 1%
* Проектирование стилей: [>--------------------------------------------------] 1%
* Приложение: Scotch Box: [>--------------------------------------------------] 1%

----------------

**Содержание**
* [Начало работы](/documentation/GettingStarted.md#part0)
  * [Что нового для разработчиков](/documentation/GettingStarted.md#part1)
  * [Начало работы](/documentation/GettingStarted.md#part2)
  * [Загрузка XF 2.0](/documentation/GettingStarted.md#part3)
  * [Системные требования XF 2.0](/documentation/GettingStarted.md#part4)
  * [Настройка локального сервера](/documentation/GettingStarted.md#part5)
    * [Предварительно построенная виртуальная машина](/documentation/GettingStarted.md#part6)
    * [Готовые сборки](/documentation/GettingStarted.md#part7)
  * [Загрузка](/documentation/GettingStarted.md#part8)
  * [Создание src/config.php](/documentation/GettingStarted.md#part9)
  * [Замечание по правам доступа к файлам](/documentation/GettingStarted.md#part10)
  * [Установка](/documentation/GettingStarted.md#part11)
  * [Переустановка](/documentation/GettingStarted.md#part12)
  * [Провека целостности файлов](/documentation/GettingStarted.md#part13)
  * [Команды управления плагинами](/documentation/GettingStarted.md#part14)
    * [Установка](/documentation/GettingStarted.md#part15)
    * [Обновление](/documentation/GettingStarted.md#part16)
    * [Перестроение](/documentation/GettingStarted.md#part17)
    * [Удаление](/documentation/GettingStarted.md#part18)
* [Структура плагина](/documentation/AddOnStructure.md#part0)
  * [Идентификаторы плагина и дополнительные надстройки](/documentation/AddOnStructure.md#part1)
  * [Рекомендации к строковому формату версии](/documentation/AddOnStructure.md#part2)
  * [Рекомендации к идентификатору версии](/documentation/AddOnStructure.md#part3)
  * [Основные файлы и директории плагина](/documentation/AddOnStructure.md#part4)
    * [Файл addon.json](/documentation/AddOnStructure.md#part5)
    * [Файл hashes.json](/documentation/AddOnStructure.md#part6)
    * [Файл Setup.php](/documentation/AddOnStructure.md#part7)
    * [Директория _data](/documentation/AddOnStructure.md#part8)
    * [Директория _output](/documentation/AddOnStructure.md#part9)
  * [Класс установки](/documentation/AddOnStructure.md#part10)
* [Инструменты разработки](/documentation/DevelopmentTools.md#part0)
  * [Режим отладки](/documentation/DevelopmentTools.md#part1)
  * [Включение режима разработки](/documentation/DevelopmentTools.md#part2)
  * [Команды разработки](/documentation/DevelopmentTools.md#part3)
  * [Дополнительные команды плагинов](/documentation/DevelopmentTools.md#part4)
    * [Создание нового плагина](/documentation/DevelopmentTools.md#part5)
    * [Экспорт .XML файлов _data](/documentation/DevelopmentTools.md#part6)
    * [Повышение версии плагина](/documentation/DevelopmentTools.md#part7)
    * [Синхронизация addon.json с базой данных](/documentation/DevelopmentTools.md#part8)
    * [Проверка файла addon.json](/documentation/DevelopmentTools.md#part9)
    * [Запустить отдельный шаг установки](/documentation/DevelopmentTools.md#part10)
      * [Запустить шаг установки](/documentation/DevelopmentTools.md#part11)
      * [Запустить шаг обновления](/documentation/DevelopmentTools.md#part12)
      * [Запустить шаг удаления](/documentation/DevelopmentTools.md#part13)
  * [Выпуск сборки плагина](/documentation/DevelopmentTools.md#part14)
    * [Расширеный процесс сборки](/documentation/DevelopmentTools.md#part15)
  * [Команды разработки](/documentation/DevelopmentTools.md#part16)
    * [Импорт вывода разработки](/documentation/DevelopmentTools.md#part17)
    * [Экспорт вывода разработки](/documentation/DevelopmentTools.md#part18)
  * [тладка кода](/documentation/DevelopmentTools.md#part19)
    * [Отладка переменной](/documentation/DevelopmentTools.md#part20)
* Общие понятия
  * Компоненты поставщика
  * Интегрированная среда разработки (IDE)
  * АвтоЗагрузчик
  * Пространства имён
  * Короткие имена классов
  * Расширение классов
  * Типовые подсказки
* Основы маршрутизации
  * Простой пример
    * Префикс маршрута
    * Раздел контекста
    * Контроллер
    * Экшен контроллера
    * Более продвинутый пример (форматы маршрута)
    * Параметры маршрута
    * Суб-имена
* Основы контроллера
  * Вывод
  * Перенапровление на префикс маршрута
  * Вывод ошибки
  * Вывод сообщения
  * Бросить исключение
  * Перенаправление на класс маршрута
  * Изменение ответа на действие контроллера (правильно)
* Сущности, Поисковики и Репозитории
  * Поисковики (Finder)
    * Метод where
    * Метод whereOr
    * Метод with
    * Метод order 
    * Метод limitByPage
    * Метод limit
    * Метод getQuery
    * Расширение методов поисковика
  * Система сущностей (Entity)
    * Структура сущностей
      * Таблица
      * Короткое имя
      * Тип контента
      * Основной ключ
      * Колонки
      * Поведения
      * Геттеры
      * Связи
      * Опции
    * Жизненный цикл сущностей
    * Репозитории (Repository)
* [Управление схемой](/documentation/SchemaManagement.md#part0)
  * [Адаптер базы данных](/documentation/SchemaManagement.md#part1)
  * [Управление схемой](/documentation/SchemaManagement.md#part2)
* Давайте построим плагин
  * Создание плагина
  * Создание класса установки
  * Расширение сущности XF:Forum
  * Расширение сущности XF:Thread
  * Создание новой сущности
  * Изменение формы редактирования форума
  * Расширение процесса сохранения XF:Forum
  * Настройка потока, которая будет отображаться автоматически
  * Создание страницы портала
  * Создание пункта навигации
  * Manually featuring (or unfeaturing) threads
  * Улучшение страницы портала
  * Создание разрешений и оптимизация
  * Создание некоторых параметров
  * Не возможно изменить видимость
  * Последние штрихи
  * Сборка плагина
* Проектирование стилей
  * Включение режима дизайнера
  * Включение режима дизайнера для стиля
  * Отключение режима дизайнера для стиля
  * Что выводится и где?
    * Шаблоны
    * Группы и параметры стилей
  * Изменение конкретного шаблона 
  * Другие полезные команды
    * Экспорт в базу данных
    * Импорт в файловую систему
    * Синхронизация шаблонов
    * Обратный шаблон
* Приложение: Scotch Box
  * Установка Scotch Box
  * Куда идут файлы?
  * Остановка и перезапуск сервера
  * Оффициальная документация
  * Scotch Box Профессионал
