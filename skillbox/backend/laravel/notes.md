# Laravel Notes

## Important notes and links

---

* Конструктор форм -
  [`laravel-form-builder`](https://packagist.org/packages/kris/laravel-form-builder)
* `twilio/sdk` - сервис СМС
* **Laravel Breeze** - Библиотека для аутентификации.
* Telegram Bot API SDK - популярный SDK для телеграм бота.
* <https://sentry.io> - багтрекер Sentry
* `composer require laravel/socialite` - расширение для OAuth

---

## 1.3 Обзор фреймворка Laravel

В этом видео объясняется структура каталогов Laravel.

## 1.4 Создание контроллера

`php artisan make:controller имя_контроллера` - Генерация контроллера

`Route::get($route, fn или массив с классом и экшеном);` - Создание пути

## 1.5 Подключение к базе данных

После изменения конфигурации необходимо выполнить команду
`php artisan config:clear`, которая очищает кэш конфига.

Для низкоуровневого доступа к базе данных используется класс `DB`.

```php
<?php

$users = DB::connection('mysql')->table('user')
->select(['first_name', 'last_name', 'email'])
->get();

print_r($users);
```

## 1.6 Подключение шаблона страницы

В парадигме Laravel шаблон это и есть представление.
В Laravel по умолчанию используется шаблонизатор Blade.

```php
<?php

return view('templateName', ['users' => $users]);
```

## 1.7 Установка пакетов, Composer

Composer позволяет добавлять пакеты из трёх источников

* packagist.org
* Git
* ZIP файла

  Основные команды Composer

* `install` - установка зависимостей из composer.json
* `update`
* `require` - установка и добавление в composer.json
* `remove`
* `dump-autoload` - пересборка автозагрузчика

## 1.8 Профайлер и его подключение

**Профайлер** — это инструмент, позволяющий
отследить состояние приложения и его
характеристики, такие как потребление памяти,
выполняемые запросы к базе данных и скорость
их выполнения, число обращений к кешу и так далее.

Для подключения профайлера необходимо установить опцию
APP_DEBUG в true в файле .env.
Также необходимо установить пакет для GUI.

Профайлер - это удобный инструмент для оптимизации и отладки приложения.

## 2.1 Понятие контроллера, виды контроллеров

Виды контроллеров в Laravel

* Обычные контроллеры
* Single action контроллеры
* Контроллеры ресурсов

## 2.3 Экшены и роутинг

```php
<?php

Route::get('/books', [BookController::class, 'index'])->name('alias');
```

Использование псевдонима позволяет ссылаться на роут в более коротком виде.

`Route::get('/books/{id?}', [BookController::class, 'delete']);`
Необязательный параметр.

```php
<?php
Route::get('/books/{id}', [BookController::class, 'delete'])
->where(['id' => $regexp]);
```

Проверка формата параметра.

## 2.4 Параметры запросов

`$file = $request->file($fileName);` - получение файла.

```php
<?php

$allHeaders = $request->header();

$specificHeader = $request->header('header-name');
```

Получение заголовков.

## 2.6 Ответ в формате JSON

`return response()->json($array);` - возвращение данных в виде JSON в
качестве ответа.

## 2.7 Редиректы и их виды

Виды редиректов в Laravel:

* Редирект на внешний ресурс
* Редирект на экшен контроллера
* Редирект на роут

  ```php
  <?php

  return redirect()->action(
      [UserController::class, 'actionName'],
      ['id' => $id]
  );
  ```

  Редирект на экшен

  `return redirect()->away('https://google.com');` - редирект на другой
  ресурс.

## 3.1 ORM-система

Модель именуется с большой буквы в единственном числе.
Соответствующая ей таблица именуется в множественном числе в snake_case.

## 3.2 Создание сущности

`php artisan make:model User` - создание сущности.

```php
<?php

class ExampleModel extends Model
{
    protected $connection = 'mysql'; // Выбрать движок базы данных.
    protected $primaryKey = 'primary-key-field'; // Задать первичный ключ
    public $incrementing = true;
    protected $table = 'table_name'; // Произвольное название таблицы
    public $timestamps = true;

    // Данные модели
    protected $attributes = ['first_name', 'last_name', 'email']; 
}
```

## 3.3 Миграции и управление ими

`php artisan make:migration create_flights_table` - создание миграции.

Миграции содержат два метода.
`up` - действия при создании БД
`down` - действия при удалении БД

Запуск всех миграций осуществляется в консоли
с помощью Artisan команды:
`php artisan migrate`
Чтобы вывести список ранее запущенных миграций:
`php artisan migrate:status`
Принудительный запуск миграций:
`php artisan migrate –- force`

Чтобы откатить миграции, воспользуемся artisan-командой, которая
возвращает пакет изменений с последнего запуска миграций:
`php artisan migrate:rollback`
Для отката определённого числа файлов миграции воспользуемся
флагом step
`php artisan migrate:rollback --step=5`
Чтобы откатить все миграции на проекте применяют reset-команду:
`php artisan migrate:reset`

## 3.4 Добавление, обновление и удаление данных

`$object->delete()` - удаление записи из базы данных

`ModelClass::truncate()` - удаление всех записей

`updateOrCreate` - обновление или создание записи.

## 3.5 Получение данных из базы

`ModelClass::all()` - получение всех записей
`ModelClass::find($keyValue)` - поиск по первичному ключу

`$employees = Employee::where('age' > 35)->get();` - поиск по условию

Для получения одной записи можно использовать методы `first()`
или `firstWhere()`

## 3.6 Сортировка и ограничение

Сортировка производится с помощью метода `orderBy('column_name', 'DESC');`
Второй аргумент это порядок сортировки.

`take()` или `limit()` методы для ограничения выборки по числу.
Эти методы одинаковы.

`skip()` или `offset()` методы для смещения данных. Эти методы одинаковы.

## 3.7 Группировка данных

Для группировки данных в Eloquent используется метод `groupBy('column_name')`.

`ModelClass::distinct()` - выборка по уникальным значениям.

## 3.8 Сложные запросы и SQL

**Причины частичного или полного применения SQL-запросов
вместо ORM на проекте.**

* Излишняя функциональность и дополнительный слой абстракции
  в виде ORM
* Более быстрая скорость выполнения запросов при работе
  с большими данными
* Избыточные или повторные запросы к базе при использовании ORM

## 4.1 Шаблон. Шаблонизаторы в PHP

**В практике php-девелопера могут встречаться разные
шаблонизаторы. Самые популярные из них**:

* [Smarty](https://github.com/smarty-php/smarty)
* [Plates](https://github.com/thephpleague/plates)
* [Mustache](https://github.com/bobthecow/mustache.php)
* [Twig](https://github.com/twigphp/Twig)
* [Blade](https://github.com/jenssegers/blade)
* [Blade One](https://github.com/EFTEC/BladeOne)
* [Latte](https://github.com/nette/latte )

## 4.2 Обзор возможностей шаблонизатора Blade

**Шаблонизатор Blade имеет возможности**:

* условной отрисовки шаблонов и их компонентов
* использования циклов и других директив для динамического
  вывода информации
* включения одних шаблонов в другие. Поддержки компонентов
  и слотов для переиспользования
* условной отрисовки css-стилей
* возможной совместимости c JavaScript-фреймворками

## 4.3 Вложенные шаблоны

`@include('templateName', ['data' => 123])` - вставка вложенных шаблонов.

Все данные родительского шаблона будут доступны и в дочернем.

`php artisan make:component ComponentName` - создание компонента

Класс компонента будет помещён в `app/View/Components`,
а шаблон — в `resources/views/components`.

Компоненты можно создавать в поддиректории.
Имя такого компонента будет:
`<x-directory-name.component-name></x-component-name>`

`<x-component literal-attribute="abc" :variable-attribute="$value" />`
Атрибуты компонентов.

`{{ $slot }}` - Содержимое компонента.

Есть возможность определить несколько слотов с помощью `x-slot`
`<x-slot name="title">Заголовок</x-slot>`

## 4.4 Blade-директивы

Часто используемые директивы:

* Директивы условной отрисовки: `@if`, `@elseif`, `@else` и `@endif`
* Циклы: `@for`, `@endfor`, `@foreach`, `@endforeach` и др.
* Директивы вставок: `@include`, `@includeIf`, `@includeWhen`

  `@php и @endphp` - Вставка PHP кода.

  `{{-- Комментарий --}}`

## 4.5 Хелперы. Работы с формами

```blade
@error('title')
<div class="alert alert-danger">{{ $message }}</div>
@enderror
```

Хелпер для ошибки формы.

Также можно создавать свои хелперы в `app/Providers`.

## 5.2 Получение параметров запроса

`$request->input('inputName', 'defaultValue');` - получение данных через
`input`

Чтобы получить данные всех полей, можно использовать `$request->all()` или
`$request->collect()`.

`all()` - возвращает данные в виде массива.
`collect()` - возвращает данные в виде коллекции.

## 5.3 Получение заголовков запроса

`$request->hasHeader('Header-Name');` - проверка существует ли заголовок.
`$request->header('Header-Name', 'defaultValue');` - получение заголовка.

## 5.4 Работа с cookie и сессиями

`$request->cookie('cookie-name');` - получение и установка cookie.
Cookie в Laravel автоматически шифруются.

`$request->session()->get('parameterName');` - Получения параметра сессии.

## 5.5 Передача файлов в запросе

`$request->file('name');` - получение объекта класса `UploadedFile`.

`<input type="file" name="images[]" accept="image/*" multiple>` - тег для
получения нескольких файлов.

`$request->hasFile('name');` - проверка наличия файла.
`$request->file('name')->isValid();` - проверка корректной загрузки файла.

Метод `path()` возвращает временный путь файла.
Метод `extension()` возвращает расширение файла.
Метод `store($path)` сохраняет файл.

## 5.6 Обработка raw JSON

Поля JSON можно получить с помощью `input`

`$request->json()->all()` - ещё один способ получить данные JSON.
`json_decode($request->getContent(), true);` - ещё один способ получить
данные JSON.

## 6.1 Элементы управления формой

Чтобы получить массив значений из тегов `<input>` необходимо указать имя с
квадратными скобками `inputName[]`.

`<input type="hidden">` - скрытый input.

## 6.4 Валидация формы

В Laravel валидация осуществляется с помощью `$request->validate()`.

Правила валидации могут быть как в виде строки, так и в виде массива.

```php
<?php

$rules = [
'title' => 'required|string|unique:employee|max:255',
'description' => 'required',
];

$arrayRules = [
'title' => ['required', 'string', 'unique:employee', 'max:255'],
'description' => 'required',
];
```

Переменная класса `Illuminate\Support\MessageBag` `$errors` доступна во
всех представлениях приложения.
Также есть директива `@error('input-name')`

## 6.5 Конструктор форм Laravel

В Laravel с 5 версии нет встроенного конструктора форм.
При необходимости можно подключить сторонний конструктор.
Например `laravel-form-builder`.

## 7.1 Класс Response в Laravel

Глобальный хелпер `response('/route')` позволяет перенаправлять запрос.

`Illuminate\Http\Response` - класс объекта ответа.

301 - Постоянный редирект.
302 - Временный редирект.

## 7.2 Установка HTTP-заголовков и cookie

Существует два способа отправить куки:

```php
<?php

return response()->cookie($name, $value, $minutes);

$cookie = cookie($name, $value, $minutes);

return response()->cookie($cookie);
```

`response()->withoutCookie('name');`
`Cookie::expire('name');`
обнуление cookie.

## 7.3 Сессии в Laravel

`$request->session()->get('key', 'default');` - получение параметра сессии.
`$request->session()->get('key', function () { return 'default'; });`
получение параметра сессии с callback.

Метод `all()` позволяет получить все данные сессии.

Также есть глобальный хелпер `session()`.

```php
<?php

// The has method returns true if the item is present & not null.
$request->session()->has('key');

// The exists method returns true if the item is present, event if its value is null
$request->session()->exists('key');

// The missing method returns true if the item is not present or if the item is null
$request->session()->missing('key');
```

## 7.4 Особенности организации REST API в Laravel

```php
<?php

// Чтобы вернуть ответ в формате JSON, воспользуемся встроенным методом
// json():
return response()->json([
  'name' => 'Abigail',
  'state' => 'CA',
]);

// Можем вернуть данные непосредственно из Eloquent-модели в формате JSON:
use App\Models\User;

Route::get('/user/{user}', function (User $user) {

  return $user;
});
```

## 7.5 Ответ сервера с файлом

`return response()->download($pathToFile, $name, $headers);` - ответ в виде файла.

Потоковая загрузка файла.

```php
<?php

// Пример использования метода streamDownload. Метод
// принимает замыкание (функцию обратного вызова), имя
// загружаемого файла и набор заголовков ответа в виде
// массива.
return response()->streamDownload(function () {
  echo 'CSV Contents...';
}, 'export.csv');
```

`return response()->file($pathToFile, $headers)` - Отображение файла в
браузере вместо загрузки.

## 8.1 Понятие сервиса

**Сервис-провайдеры** — это основной механизм загрузки всех приложений
Laravel. Любое приложение, а также все основные службы и сервисы Laravel
загружаются сервис-провайдерами

Массив провайдеров находится в конфиге `app.php`.

Сервис провайдер может быть с обычной загрузкой (загружается при каждом
запросе) и отложенной (загружается только при надобности).

Отложенный провайдер реализует интерфейс `DeferrableProvider`.

## 8.3 Сервис-провайдеры

В контексте сервис контейнеров ключевым процессом является
связывание. Под ним подразумевается связывание зависимостей с
конкретным классом.

`$this->app->bind(EventPusher::class, RedisEventPusher::class);`

Привязывание контекста.

```php
<?php

$this->app->when(PhotoController::class)
    ->needs(Filesystem::class)
    ->give(function () {
        return Storage::disk('local');
    });

$this->app->when([VideoController::class, UploadController::class])
    ->needs(Filesystem::class)
    ->give(function () {
        return Storage::disk('s3');
    });
```

Привязка примитивов.

```php
<?php

$this->app->when('App\Http\Controllers\UserController')
    ->needs('$variableName')
    ->give($value);

// Если вам нужно внедрить значение из одного из конфигурационных 
// файлов вашего приложения, то вы можете использовать метод giveConfig:
$this->app->when(ReportAggregator::class)
    ->needs('$name')
    ->giveConfig('app.name');
```

Сервисы можно группировать с помощью метки (tag)

`$transistor = $this->app->make(Transistor::class);`
Извлечение сервиса. Оно позволяет создать сервис без конструктора
и аргумента в экшене.

`$report = App::call([new UserReport, 'generate']);`
Вызов и внедрение зависимостей.

Обработка событий

```php
<?php

$this->app->resolving(Transistor::class, function ($transistor, $app) {
    // Code
});
```

If some of your class's dependencies are not resolvable via the
container, you may inject them by passing them as an associative
array into the `makeWith` method.

`$transistor = $this->app->makeWith(Transistor::class, ['id' => 1]);`

## 8.4 Работа с базой данных внутри сервиса

`php artisan make:provider ServiceName` - создание провайдера сервиса.

Провайдер нужно добавить в конфиг `app.php`.

Сервисы можно создать в папке `app/Services/` или в её подпапках.

Передача зависимости БД при регистрации сервиса.

```php
<?php

$this->app->bind(CustomLogServiceInterface::class, function () {
    return new CustomLogDbService(DB:table('tableName'));
});
```

## 8.5 Создание собственного сервиса

`twilio/sdk` - сервис СМС

## Middleware

`php artisan make:middleware` - создание middleware

Глобальные или групповые middleware регистрируется `app/Http/Kernel.php`
Также в `kernel` можно зарегистрировать middleware как псевдоним.

```php
<?php

// Добавление middleware к отдельному роуту
Route::get('/', [ExampleController::class, 'index'])
    ->middleware(ExampleMiddleware::class);

```

`->withoutMiddleware('middlewareName');` - исключение middleware

The `withoutMiddleware` method can only remove route middleware and
does not apply to global middleware.

Middleware можно регистрировать в контроллере.

Метод `terminate` вызывается после отправки реквеста браузеру.

```php
<?php
 
namespace Illuminate\Session\Middleware;
 
use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;
 
class TerminatingMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Closure(\Illuminate\Http\Request):
     * (\Symfony\Component\HttpFoundation\Response) $next
     */
    public function handle(Request $request, Closure $next): Response
    {
        return $next($request);
    }
 
    /**
     * Handle tasks after the response has been sent to the browser.
     */
    public function terminate(Request $request, Response $response): void
    {
        // ...
    }
}
```

## 9.1 Понятия «событие» (Event) и «слушатель» (Listener) в Laravel

**Событие** - это **сообщение**, которое возникает в различных
точках исполняемого кода при выполнении определённых условий.

Оно позволяет отделить логику события от логики его реализации.

Слушатель - это **класс**, содержащий логику обработки события.
Вы можете создавать неограниченное количество слушателей на одно событие.

Жизненный цикл моделей

Создание->Обновление->Сохранение->Удаление

## 9.2 Создание события и слушателя

Алгоритм создания события

* Создать класс «событие»
* Создать класс «слушатель»
* Осуществить привязку события к слушателю

`php artisan make:event EventName` - создание события.

`php artisan make:listener ListenerName --event=EventName` -
создание слушателя.

`EventName::dispatch($parameters);` - Запуск события.

События привязываются в `EventServiceProvider`.

`php artisan event:generate` - создает события и слушатели
зарегистрированные в `EventServiceProvider`.

Event discovery is disabled by default, but you can enable it by
overriding the `shouldDiscoverEvents` method of your application's
`EventServiceProvider`.

## 9.4 Работа с классом «слушатель»

### Возможности слушателей

* Возможность запускать слушатели в очереди
* Возможность прерывать порядок выполнения очередей
* Позволяет автоматически внедрять в класс все необходимые
  зависимости в коде конструктора класса

Для прерывания цепочки слушателей нужно вернуть false.

Sometimes, you may want to instruct Laravel to only dispatch an
event after the active database transaction has committed. To do
so, you may implement the `ShouldDispatchAfterCommit` interface on
the event class.

## 9.5 Встроенные события моделей

Обработчики событий жизненного цикла моделей определяются внутри модели.

`ModelName::withoutEvents($callback);` - Операции над моделью без событий.

## 9.6 Наблюдатели и их использование

Наблюдатель - это класс, позволяющий сгруппировать логику обработки
событий модели в одном месте.
Наблюдатель содержит в себе методы, названия которых идентичны
названиям событий моделей.

`php artisan make:observer --model=ModelName` - создание наблюдателя.

Наблюдатель необходимо передать в `EventServiceProvider` в функцию
`boot` в виде `User::observe(UserObserver::class);`.

Чтобы сохранить, обновить, удалить и т.д. модель без вызова событий
нужно использовать один из методов `saveQuietly`, `deleteQuietly`,
`updateQuietly` и т.д.

## 10.2 Очереди в Laravel

Очереди необходимы для снятия нагрузки с основного процесса и ускорения
ответа на запрос.

Значения `QUEUE_CONNECTION`:

* `null` - задачи очереди не выполняются
* `sync` - задачи в очереди выполняются немедленно т.е.
  синхронно.

Другие возможные значения:

* `database` - Uses a database table to store queued jobs. This is useful for
  simple applications without external queue services.
* `redis` - Leverages Redis for efficient job processing. This is a great
  option for high-performance applications with many queued jobs.
* `beanstalkd` - Uses `Beanstalkd`, a simple and fast work queue. This is
  suitable for applications that need a lightweight queuing system.
* `sqs` - Amazon Simple Queue Service (SQS), a fully managed message queuing
  service offered by AWS. This is ideal for cloud-based applications needing
  scalable queue solutions.
* `iron` - `IronMQ`, a cloud-based message queuing service. This is another
  option for scalable applications.
* `pusher` - Used for broadcasting events via Pusher, which can be integrated
  into the queue system for real-time applications.
* `custom` - You can also define your own custom queue driver if you need a
  specialized solution.

`php artisan make:job JobName` - создание новой задачи.

`JobName::dispatch($data);` - вызов задачи.

Действия для установки асинхронной очереди.

1. `php artisan queue:table`
2. `php artisan migrate`
3. Установить `QUEUE_CONNECTION` в значение `database`
4. `php artisan queue:listen`

## 10.3 Планировщик задач

Для запуска задач необходима одна запись в **cron**.
`php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1`

`php artisan schedule:list` - просмотр задач.

Способы создания задачи:

* Использование замыкания
* Выполнение консольных команд фреймворка
* Планирование задач в очереди
* Выполнение команды оболочки

`php artisan make:command CommandName` - создание консольной команды

Регистрация задач

```php
<?php

protected function schedule(Schedule $schedule)
{
    $schedule->call(function () {
        Log::info('Callback executed');
    })->everyMinute();

    $schedule->command('app:command-name')->everyMinute();
    $schedule->job(JobName::class)->everyMinute();
    $schedule->exec('echo "Command executed" >> storage/logs/test.log')
        ->everyMinute();
}
```

`php artisan schedule:work` - другой способ запустить планировщик задач.

## 10.4 Локализация

`App::getLocale();` - получить текущую локализацию.
`App::setLocale('ru');` - поставить локализацию.
`__('file.string');` - Получить локализованный текст.

В текст можно передавать параметры.

## 10.5 Нотификация Laravel

Встроенные каналы распространения:

* Почта
* SMS
* Slack
* SQL

`php artisan notification:table` - создание миграции уведомлений.
`php artisan make:notification NotificationName` - создание уведомления.

В функции `toArray` заполняются необходимые для уведомления данные.

`$user->notify(new NotificationName($data));` - запуск уведомления.

`$user->notifications` - получение всех уведомлений пользователя.

## 11.1 Авторизация и аутентификация

**Различие терминов:**
Аутентификация -> Кто это?
Авторизация -> Что ему можно делать?

**Laravel Breeze** - Библиотека для аутентификации.

Установка:

* `composer require laravel/breeze`
* `php artisan breeze:install`
* `npm install && npm run dev`

## 11.3 Классы и посредники в Laravel

`php artisan make:middleware MiddlewareName` - создание посредника.

## 11.4 Авторизация в Laravel

**Шлюз авторизации** - это функция-замыкание, которая возвращает
разрешение на выполнение действия пользователю.
Она определяется в классе `AuthServiceProvider`, в методе `boot`.

```php
<?php

Gate::define('view-users', function (User $user) {
    return $user->isAdmin();
});

// Inside controller
Gate::authorize('view-users');
```

**Политики** — это специальные классы, которые предоставляют методы
авторизации (разрешения) действий пользователя.

`php artisan make:policy PolicyName --model=User` - создание политики.

`$this->authorize('methodName', $data);`

## 11.5 Ролевая модель

`php artisan make:model Role -m` - создание модели роли.
`php artisan make:migration update_users_add_role_id --table=users` -
добавление роли для пользователей.

## 12.2 Интеграция с внешними сервисами

С помощью интеграций можно существенно расширить функциональность своего
ресурса.

* Мессенджеры: `Slack`, `Telegram`, `WhatsApp`
* Системы аутентификации: `Google OAuth`, `Github`, `ВКонтакте`
* Опросы: `Typeform`, `Form.io`
* Платежные провайдеры: `Юмани`, `Qiwi`
* Видеохостинги: `Youtube`, `Vimeo`
* Системы отслеживания ошибок
* И другие

## 12.3 Почтовые сервисы

Настройки почтовой интеграции находятся в файле `.env`.

`php artisan make:mail MailingName` - создать класс для создания писем.

`return $this->subject($subject)->view('emails.view-name');` - Создание
письма внутри класса с помощью view.

`Mail::to($recieverEmail)->send(new MailingName());` - отправка письма.

## 12.4 Интеграция мессенджера

Telegram Bot API SDK - популярный SDK для телеграм бота.

`BotFather` - телеграм бот для создания бота.

## 12.5 Интеграция сервиса отслеживания ошибок

<https://sentry.io> - багтрекер Sentry

## 12.6 OAuth-аутентификация

`composer require laravel/socialite` - расширение для OAuth

## 13.1 Вводная лекция

**Задачи тестирования**:

* Проверка функциональности
* Выявление ошибок
* Упрощение разработки
* Улучшение качества

Laravel предоставляет множество инструментов для тестирования приложений,
таких как `PHPUnit`, `Laravel Dusk`, `Laravel Browser Kit` и других.

### Фабрики данных

Как **автоматизированно** заполнить базу данных тестовыми значениями?
Laravel имеет **простой** метод заполнения базы данных тестовыми данными,
используя **классы-наполнители** и **классы-фабрики**.

## 13.2 XDebug

**Преимущества**:

* обладает удалённым отладчиком
* позволяет получить информацию о «покрытии кода»
* предоставляет функционал «трассировки стека»

```bash
pecl install xdebug # установка XDebug.
php -v # проверка установки
php --ini # нахождение конфигурационного файла
```

В php.ini нужно добавить:

```ini
zend_extension="xdebug.so"
xdebug.mode=debug
```

## 13.3 Фабрики данных

Классы **Seeder**, или **классы-наполнители**, — это классы, которые
отвечают за наполнение базы данных определёнными сущностями.

Классы **Factory**, или **классы-фабрики**, — это встроенные в
фреймворк Laravel классы, которые используются для создания
экземпляров класса вашей модели.

`php artisan make:seeder SeederName` - создание наполнителя.

`php artisan make:factory FactoryName` - создание фабрики.

Для создания данных используется библиотека Faker.

`php artisan db:seed` - вызов наполнителей.

`php artisan db:seed --class=SeederName` - вызов определенного наполнителя.

## 13.5 Тестирование приложений в фреймворке Laravel

Laravel имеет каталог для Unit и Feature тестов.

**Feature тесты** (или **функциональные тесты** - нацелены на охват большей
части кода системы.

**Функциональное тестирование** — тестирование некого функционала продукта,
при этом продукт воспринимается как единый «чёрный ящик».

`php artisan make:test FolderName/TestName` - создание функционального теста

`php artisan test` - запустить тесты.

## 14.1 Создание администраторской панели

**Готовые решения для административной панели:**

* Laravel Nova
* Laravel Backpack
* Admin LTE
* Laravel Voyager

## 14.2 Интеграция Voyager

**Преимущества:**

* гибкость и настраиваемость
* широкий набор функциональных возможностей
* простота использования
* быстрая интеграция
* поддержка сообщества

**Недостатки:**

* Ограниченность функциональности
* Ограниченная настройка внешнего вида
* Проблемы совместимости

Установка Voyager:
`composer require tcg/voyager`
`php artisan voyager:install`

`php artisan voyager:admin your@email.com --create` - добавление админа

Для отображения картинок и медиафайлов в админке нужно прописать `APP_URL` в .env.

## 14.3 Настройка дашборда, хранилище медиа и использование переменных

В voyager.php можно добавлять виджеты. Также можно создавать свои виджеты.

Voyager имеет коллекцию готовых иконок.

BREAD - аналог CRUD в Voyager.

## Eloquent ORM

When issuing a mass update via Eloquent, the saving, saved, updating,
and updated model events will not be fired for the updated models.
This is because the models are never actually retrieved when issuing a
mass update.

Soft deleting models will be permanently deleted (`forceDelete`) if
they match the prunable query.

If your global scope is adding columns to the select clause of the query,
you should use the `addSelect` method instead of select. This will prevent
the unintentional replacement of the query's existing select clause.

Parent model timestamps will only be updated if the child model is updated
using Eloquent's save method.

**Mass assignment** protection is automatically **disabled** when creating
models using factories.

## Migrations

The migrate:fresh command will drop all database tables regardless
of their prefix. This command should be used with caution when
developing on a database that is shared with other applications.
