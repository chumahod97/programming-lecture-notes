# Symfony Notes

## Important notes and links

---

---

## 1.1 Введение и Установка Symfony

`symfony check req` - проверка Symfony перед запуском  
`symfony serve` - запуск DEV сервера Symfony  
`symfony serve -d` - запуск в фоновом режиме  
`symfony server:status` - статус сервера  
`symfony server:stop` - остановка сервера

Symfony можно настроить на https.

## 1.3 Создание первых страниц, маршрутизация и контроллеры

`composer require annotations` - пакет аннотаций для Symfony.

## 1.4 Flex и Recipes

`composer require sec-checker` - установка проверки безопасности  
`php bin/console security:check` - проверка пакетов на уязвимости  
`composer remove sec-checker` - удаление пакета

## 1.5 Шаблонизатор Twig

`composer require twig` - установка шаблонизатора

## 1.6 Debug Profiler и другие инструменты отладки

`composer require profiler --dev` - пакет профайлера  
`composer require debug` - установка отладчика  
`composer unpack symfony/debug-pack` - распаковка пакета с зависимостями

## 1.7 Подключаемые файлы: CSS и Js

`href={{ asset('css/app.css') }}` - подключение статичных файлов.

`{{ parent() }}` - родительский блок.

## 1.8 Генерация URL

`<a href="{{ path('app_homepage') }}" >` - указание пути в ссылке.

## 1.9 Создание JSON-API

`return new JsonResponse(compact('likes'))` - вывод данных в формате JSON.
`return $this->json(compact('likes))` - укороченный способ.

## 1.10 Сервисы и Autowiring

Сервисы - это объекты которые делают полезную работу.

`php bin/console debug:autowiring` - получить список сервисов.

## 1.11 Webpack Encore

`composer require encore`

`yarn run watch` - запуск сборщика скриптов

`{{ encore_entry_link_tags('app') }}` - добавление CSS  
`{{ encore_script_tags('app') }}` - подключение скриптов

`yarn add bootstrap --dev` - добавление пакетов

## 2.1 Основы Autowiring и конфигурирования

Bundles - система плагинов в Symfony

`config/bundles.php` - файл конфигурации бандлов

`composer require demontpx/parsedown-bundle` - парсинг Markdown

`php bin/console config:dump` - вывод конфигураций сервисов по умолчанию  
`php bin/console config:dump alias` - вывод конфигураций пакета по умолчанию

Свой конфиг можно добавить в `config/packages/alias.yaml`  
Название конфига может быть произвольным.

`php bin/console debug:config alias` - вывод текущей конфигурации сервиса
`php bin/console cache:clear` - сброс кэша внутри приложения

`php bin/console debug:container cache.adapter.filesystem`  
`php bin/console debug:container --show-hidden` - просмотр всех сервисов  
в контейнере

## 2.6 Автовызов методов сервиса, аннотация @required

Аннотация @required помечает метод контроллера для вызова перед экшеном.
В параметрах такого метода можно указывать зависимости.

```php
<?php

/**
 * @required
 */
```

Также аннотацию можно вынести в трейт.

## 2.7 Генерация кода: symfony/maker-bundle

`composer require maker --dev` - установка бандла maker, который позволяет  
автоматически создавать классы

## 2.8 Пример создания консольной команды

`php bin/console make:command app:article-statistics` - создание консольной  
команды

## 2.9 Безопасное хранение секретных конфигураций, Vault

`php bin/console secrets:set ENV_VARIABLE_NAME` - установить секретное  
значение

## 3.1 Работа с блоками Twig

Для установки версии PHP проекта нужно создать файл .php-version с номером  
версии внутри.

К параметру запроса можно обращаться из Twig напрямую.

Оператор for имеет else который выполняется в случае пустого массива.

## 3.2 Расширения шаблонизатора Twig

`{{ someString|u.truncate(lettersCount, '...') }}` - укорачивание текста
`{{ comment.createdAt|date('d.m.Y H:i:s') }}` - форматирование даты

`php bin/console make:twig-extension AgoExtension` - создание своего фильтра  
Twig.

## 3.3 LazyLoad в расширениях

В конструкторе расширения можно указать свои зависимости.

Для ленивой загрузки нужно перенести метод расширения в класс `AppRuntime`  
который должен реализовывать интерфейс `RuntimeExtensionInterface`.

## 4.1 Знакомство с Doctrine и миграциями

`composer require doctrine` - установка doctrine

`php bin/console debug:config doctrine` - просмотр всех параметров  
конфигурации doctrine

Нужно указать версию БД для doctrine либо в конфиге либо в URL.

`php bin/console doctrine:create:database` - создать базу данных  
`php bin/console make:entity` - создать класс сущность или обновить  
существующую.

`php bin/console make:migration` - создание миграции  
`php bin/console doctrine:migrations:migrate` - запуск миграции БД  
`php bin/console doctrine:migrations:status` - статус миграции

```php
<?php

$entityManager->persist($newEntity); // Save entity in DB
$entityManager->flush(); // Commit saves to DB

$repository = $entityManager->getRepository(Article::class);

$article = $repository->find($id);
$articles = $repository->findAll($id);

$articles = $repository->findBy(['property' => $value]);
$article = $repository->findOneBy(['property' => $value]);
```

Для получения сущностей по сложным запросам используется `queryBuilder`.

## 4.3 Обновление Entity

`php bin/console doctrine:query:sql "TRUNCATE TABLE article"` - запрос в  
БД из консоли с помощью Symfony

## 5.1 Фикстуры и демоданные

`composer require orm-fixtures --dev` - установка пакета для фикстур  
`php bin/console make:fixtures` - создание фикстуры  
`php bin/console doctrine:fixtures:load` - наполнение базы данных

`composer require fzaninotto/faker --dev` - библиотека для заполнения  
mock данными

## 5.2 Расширение возможностей, Sluggable и Timestampable

`StofDoctrineExtensionsBundle` - библиотека с дополнительным функционалом  
для Doctrine

Сброс базы данных на dev:

```bash
php bin/console doctrine:database:drop --force &&
php bin/console doctrine:database:create &&
php bin/console doctrine:migrations:migrate &&
php bin/console doctrine:fixtures:load
```

## 6.1 Связь один ко многим (One-to-Many)

При создании связи нужно создать поле не article_id, а просто article.  
Тип связи должен быть relation.

## 6.2 Получение связанных объектов с дополнительными критериями

Для увеличения производительности при запросе к БД можно использовать  
`fetch="EXTRA_LAZY"`.

```php
<?php

/**
 * @ORM\OneToMany(
 *   targetEntity=Comment::class, 
 *   mappedBy"article", 
 *   fetch="EXTRA_LAZY"
 *)
*/
```

Для выборки данных в сущности нужно использовать **Criteria**.

```php
<?php

public function getNonDeletedComments(): Collection
{
    $criteria = Criteria::create()
        ->andWhere(Criteria::expr()->isNull('deletedAt')
        ->orderBy('createdAt', 'DESC')
    ;

    return $this->comments->matching($criteria);
}
```

Для включения мягкого удаления в сущности нужно использовать trait  
и добавить аннотацию.

Для мягко удаленных сущностей можно включить фильтр в doctrine.yaml

```yaml
doctrine:
    orm:
        filters:
            softdeleteable:
                class: Gedmo\SoftDeleteable\Filter\SoftDeleteableFilter
                enabled: true
```

Для решения проблемы N + 1 нужно добавить `innerJoin` и `addSelect` в запрос.

## 7.2 Постраничная навигация

`KnpLabs/KnpPaginatorBundle` - бандл для пагинации

`{{ knp_pagination_render(pagination) }}` - добавление пагинации на страницу

## 7.3 Связь Многие-ко-многим

Если нужно хранить дополнительные данные для связей, стандартная связь  
многие-ко-многим не подходит. Для этого необходимо создать отдельную  
сущность со связями один-ко-многим.

## 8.1 Создание модели пользователя

`composer require security` - установка компонента аутентификации и  
авторизации

`php bin/console make:user` - создание сущности пользователя

## 8.2 Форма авторизации

`php bin/console make:auth` - создание механизма аутентификации

## 9.1 Роли и доступы к страницам

Доступ к страницам можно настроить с помощью:

* Конфигурационного файла
* Метода в контроллере
* Аннотации в контроллере для одного метода или для всего класса

`{% if is_granted('ROLE_USER') %}` - проверка роли в twig

Строки проверки помимо роли пользователя:

* `IS_AUTHENTICATED_ANONYMOUSLY` - пользователь может быть не аутентифицирован
* `IS_AUTHENTICATED_FULLY` - пользователь аутентифицирован, но не с помощью  
  remember me токена
* `IS_AUTHENTICATED_REMEMBERED` - пользователь может быть аутентифицирован  
  и с помощью remember me токена

## 9.2 Получение авторизованного пользователя

`$user = $this->getUser()` - получение пользователя в контроллере

`{{ app.user }}` - получение пользователя в twig

## 10.1 Создание токена и API маршрута

`composer require serializer` - установка пакета для сериализации

## 11.2 Расширенное управление доступом, классы Voter

`php bin/console make:voter ArticleVoter` - создание  класса Voter

## 12.1 Подключение, вывод и обработка формы

`composer require form` - подключение пакета компонент форм

`php bin/console make:form ArticleFormType` - создание компонента формы

Добавление формы в шаблон

```twig
{{ form_start(articleForm) }}
  {{ form_widget(articleForm) }}
{{ form_end(articleForm) }}
```

## 13. Валидация в формах

`composer require validator` - установка пакета валидации

## 14.4 Кастомная валидация

`php bin/console make:validator` - создание своего кастомного валидатора

## 15. Загрузка файлов в Symfony

`composer require liip/imagine-bundle` - пакет для уменьшения изображений

`composer require oneup/flysystem-bundle` - бандл для работы с файловой  
системой

## 16.1 Отправка email в Symfony

`composer require mailer` - установка пакета для отправки email

создание нового письма

```php
<?php

$email = new Email()
    ->from('sender@example.com')
    ->to($user->getEmail())
;
```

`MailerInterface` - интерфейс для зависимости mailer

```php
<?php

$email = (new TemplatedEmail())
    ->htmlTemplate('email/welcome.html.twig');
```

Для генерации абсолютной ссылки в twig необходимо использовать  
`url('app_homepage')` вместо `path('app_homepage')`, и `absolute_url`  
вместо asset.

`$email->from(new Address('user@example.com', 'sender name'));` - добавление  
имени пользователя к адресу

## 16.2 Оформление и css в email без "боли"

`composer require league/html-to-markdown` - плагин для преобразования html.  
Применяется сразу после установки в рассылке писем.

`<img src="{{ email.image('@images/image.jpg') }} >` - добавление картинки  
во внутрь письма.

Для twig необходимо добавить путь к картинкам.

```yaml
twig:
    paths:
        'assets/images': images
```

`composer require twig/cssinliner-extra` - расширение для встроенных стилей

Включение встроенных стилей для страницы.

```twig
{% apply inline_css(source('@css/email/email.css') %}
    {# content #}
{% endapply %}
```

## 16.3 Дополнительные возможности при отправке писем

`php bin/console make:command WeeklyNewsletterCommand` - создание команды

`url('app_homepage')` - аналогично path, но глобально

Параметры для создания глобальной ссылки в services.yaml

```yaml
parameters:
    router.request_context.scheme: '%env(SITE_BASE_SCHEME)%'
    router.request_context.host: '%env(SITE_BASE_HOST)%'
```

```env
    SITE_BASE_SCHEME=https
    SITE_BASE_HOST=localhost:8000
```

## 17.1 Система событий. Слушатель

Слушатели должны быть расположены в директории `./src/EventListener`.

Подписаться на событие можно в файле конфигурации `services.yaml`.

```yaml
services:
    App\EventListener\ExampleEventListener:
        tags:
            - { name: kernel.event_listener, event: kernel.request, 
            method: 'customMethodName' }
```

`php bin/console make:subscriber` - создание подписчика

В подписчике можно задать приоритет для выполнения запроса.

```php
<?php
public static function getSubscribedEvents(): array
{
    return [
            ['onKernelResponsePre', 10],
            ['onKernelResponsePost', -10],
    ];
}
```

`php bin/console debug:event-dispatcher` - посмотреть все доступные события  
на которые подписан хотя бы один обработчик.

## 17.2 Создание и вызов своих событий

События создаются в папке `./src/Events`.

Класс должен наследоваться от класса `Event`.

## 17.3 Подзапросы

Вызов подзапроса в twig

```twig
{{ render(controller('App\\Controller\\PartialController::lastComments')) }}
```

Подзапросы плохо влияют на производительность.

## 18.1 Выделение сервиса в Bundle

Код бандла нужно перенести в `./lib/ArticleContentProviderBundle/src/`.

Пример пространства имен  
`namespace CatCasCarSkillboxSymfony\ArticleContentProviderBundle`
