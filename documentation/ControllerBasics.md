# <a name="part0"></a>Основы контроллеров
На базовом уровне контроллеры-это код, который выполняется при посещении страницы в XF. Контроллеры обычно отвечают за обработку пользовательского ввода и передачу этого ввода в соответствующее место, которое, как правило, должно выполнять какое-либо действие (модель) базы данных или загружать визуальное содержимое (представление).

Когда пользователь переходит по ссылке, запрошенный URL-адрес направляется на определенный контроллер и действие контроллера. Смотрите [Основы маршрутизации](/documentation/RoutingBasics.md#part0). Например, в XF, если посетить URL-адрес, как `index.php?conversations/add` вы будете направлены на контоллер XF`\Pub\Controller\Conversation` и на метод(action) `add`.

Если вы посмотрите на этот класс в файловой системе (see [АвтоЗагрузчик](/documentation/GeneralConcepts.md#part3) for a description of how classes and file paths map to each other) вы заметите, что методы именуются с префиксом `action`. Все эти методы указывают на определенное действие контроллера. Таким образом, чтобы увидеть метод, участвующий при просмотре страницы `conversations/add` page mentioned above, смотрите `public function actionAdd()`.

Контроллеры XF возвращают объект ответа, который может быть следующих типов:

## <a name="part1"></a>Ответ "Представление(view)"
Это один из наиболее распространенных ответов, с которым вы будете иметь дело во время разработки XF. Контроллер, который возвращает ответ представления(view), обычно требует передачу трех аргументов. Класс представления (подробнее об этом ниже), имя шаблона и массив `$viewParams` - это данные, которые будут доступны в шаблоне.

Это пример действия контроллера, который возвращает ответ представления:
```php
public function actionExample()
{
    $hello = 'Hello';
    $world = 'world!';

    $viewParams = [
        'hello' => $hello,
        'world' => $world
    ];
    return $this->view('Demo:Example', 'demo_example', $viewParams);
}
```
Первый аргумент - это короткое имя класса для определенного класса представления. Этот класс может существовать или не существовать (часто он не должен существовать, мы рассмотрим классы представлений позже), но он должен иметь уникальное имя для контроллера и действия. As with other [Короткие имена классов](/documentation/GeneralConcepts.md#part5), the particular short class name above will resolve to `Demo\Pub\View\Example`. Again, `Pub` is inferred automatically from the controller type.

Второй аргумент - имя шаблона. В этом случае мы ищем шаблон с именем `demo_example`.

Третий аргумент представляет собой массив параметров/переменных шаблона, которые доступны представлению(view). Этот массив должен быть парой `key => value` (`ключ => значение`). В приведенном выше примере в шаблон передаются два параметра. Ключ (`key`) указывает имя переменной, доступной в шаблоне. Значение массива (`value`) указывает на значение параметра.

Итак, если бы в шаблоне `demo_example` было следующее содержимое:

```php
{$hello} {$world}
```
Шаблон выведет следующее:

```Hello world!```
## <a name="part2"></a>Ответ "Перенаправление"
This reply is returned when you wish to redirect a user to a different URL after they have completed some sort of action.

A common use case here is after a user has submitted data through a form you may wish to redirect them to a different page, for example returning a user to a list of items.

Here's an example of a typical controller action that performs a redirect:
```php
public function actionRedirect()
{
    return $this->redirect($this->buildLink('demo/example'), 'This is a redirect message.', 'permanent');
}
```
The first argument is the URL to redirect to. This example will redirect the user to the `index.php?demo/example` URL.

The second argument will only display if the form is submitted over an AJAX request which opts to prevent redirecting. The result will be a "flash message" which appears from the top of the screen with your chosen message. You do not have to supply your own message. If it is not provided it will default to "Your changes have been saved".

The third argument defaults to `temporary`, but you can also opt to set this to permanent as per the example. The only difference here is the type of HTTP response code provided by the server. Temporary is ideal in most cases, and this will respond with a 303 code. `permanent` will issue a 301 response code.

Although you can trigger a permanent redirect in this way, there's actually a specific method for this, which can be used as follows. It also takes a 'message' argument, but as above it is optional.

```php
public function actionRedirect()
{
    return $this->redirectPermanently($this->buildLink('demo/example'));
}
```
## <a name="part3"></a>Ответ "Ошибка"
As the name suggests, this reply is what you will return if you need to display an error to the user. It's somewhat simple, here's an example:
```php
public function actionError()
{
    return $this->error('Unfortunately the thing you are looking for could not be found.', 404);
}
```
There are only two arguments supported here. The first is the error message you want to display, and the second is the HTTP response code you want the server to send. 404 would represent an appropriate response when something was not found.

## <a name="part4"></a>Ответ "Сообщение"
This reply is very much similar to the error reply, and supports the same arguments. The main difference is, in terms of appearance, the message displayed is not presented as an error.

## <a name="part5"></a>Ответ "Исключение"
It is sometimes necessary to interrupt the normal flow of your controller code, and reply with an Exception instead. Exception replies do not necessarily have to represent an error; for example, they can be used to force your controller to perform a redirect. However, typically, they will often be used to halt the flow of your controller to display an error, as in the following example:
```php
public function actionException()
{
    throw $this->exception($this->error('An unexpected error occurred'));
}
```
Exception replies only accept a single argument, and actually that argument must be some other form of Reply object, such as an [Ответ "Ошибка"](/documentation/ControllerBasics.md#part3). This particular example throws an exception, and the entire controller code at that point will stop, and a standard error will be displayed.

Note that exception replies must be "thrown" using `throw` rather than being "returned" with `return`.

## <a name="part6"></a>Ответ "Перенаправление маршрута"
Under certain conditions, it is necessary to reroute a user to an entirely different controller or action within the same controller, without performing a full redirect, without changing the URL the user has landed on, and without having to duplicate the code of the target action.

That looks a little bit like this:
```php
public function actionReroute()
{
    return $this->rerouteController(__CLASS__, 'error');
}

public function actionError()
{
    return $this->error('Oops! Something went wrong!');
}
```
In this particular example, if a user navigated to the `index.php?demo/reroute` URL, they would see the error reply from the `actionError()` method. They would not be redirected, nor would the URL in their browser change; they would simply just receive the reply from the error action.

The reroute reply also supports a third argument which allows various parameters to be passed from one controller action to the other. This can either be an array or a `ParameterBag` object (more on that later).

## <a name="part7"></a>Изменение ответа на действие контроллера (правильно)
In the [Расширение классов](/documentation/GeneralConcepts.md#part6) section, we've already seen how simple it is to extend a class, but extra care needs to be taken when extending a controller action that already exists.

Unless you have a specific need to override an existing action entirely, and replace it with something new (which is generally not recommended), instead you should be modifying the existing reply of the parent class. That is done quite simply, as an example let's modify the view reply from the [Ответ "Просмотр"](/documentation/ControllerBasics.md#part1) example above.
```php
public function actionExample()
{
    $reply = parent::actionExample();

    return $reply;
}
```
Assuming the above is added to an extended controller where the `actionExample()` method already exists, the above doesn't actually do anything other than return the original view reply. Let's now change the existing `hello` parameter to read "Bonjour" instead of "Hello".
```php
public function actionExample()
{
    $reply = parent::actionExample();

    if ($reply instanceof \XF\Mvc\Reply\View)
    {
        $reply->setParam('hello', 'Bonjour');
    }

    return $reply;
}
```
Because a controller reply can actually represent a number of different objects that have different behaviors and methods, it is imperative that we only attempt to extend the correct reply type. We do that in the example above by checking to see if the parent `$reply` object is actually a `View` type. If we didn't do this, we extended this action and the controller action replies with a redirect instead, then there would likely be an error.

Before extending this action visiting this page would display "Hello world!". After extending it, the view will now display "Bonjour world!".
