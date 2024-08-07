# Node.js

## Important notes and links

---

* [Простая библиотека для WebSockets `ws`](https://github.com/websockets/ws)
* [Альтернатива `ws` `uWebSockets.js`](https://github.com/uNetworking/uWebSockets.js)

---

## 4.1 Документация Node.js, globals

`process.env` - переменные окружения  
`process.env.NODE_ENV` - вид среды (разработка, продакшн)  
`process.argv` - аргументы программы  
`process.exit()` - выход из программы

## 4.2 Файловая система

```typescript
const fs = require('fs');

// Синхронная запись файла
fs.writeFileSync('test.txt', 'contents', 'utf8');

// Синхронное чтение файла
console.log(fs.readFileSync('test.txt', 'utf8');

// Асинхронная запись файла
fs.writeFile('test.txt', 'contents', 'utf8', callback);

// Асинхронное чтение файла
console.log(fs.readFile('test.txt', 'utf8', callback);

// Проверка файла
fs.access(path, callback);
```

* `fs.mkdir` - создание директории
* `fs.rmdir` - удаление директории
* `fs.unlink` - удаление файла
* `fs.appendFile` - добавление текста в файл
* `fs.copyFile` - копирование файла

`path.join(__dirname, 'filename.txt');` - для создания пути файла нужно  
использовать библиотеку `path`.

`path.resolve(__diranme, '..', filename.txt');` - определить путь

## 4.3 HTTP-сервер

Парсинг URL

```typescript
const parsedUrl = new URL(req.url, serverName);

parsedUrl.pathname; // Путь URL
parsedUrl.searchParams; // GET параметры
```

`querystring` - ещё один модуль для парсинга параметров

Стандартные методы для работы с кодировкой спецсимволов URL:  
`encodeURIComponent`  
`decodeURIComponent`  
`encodeURI`  
`decodeURI`

## 4.4 Полезные сторонние модули

На что обращать внимание при выборе модуля:

* Популярность
* Число и динамика issues
* Качество документации
* Качество кода

Список рекомендуемых модулей:

* `ms` - конвертация времени в/из миллисекунд
* `nanoid` - генерация случайных id
* `moment` (большая) и `data-fs` (маленькая) - работа с датой и временем
* `dotenv` - переменные окружения
* `cross-env` - исправляет npm scripts на Windows

Не стоит устанавливать маленькие модули для проблем решаемых парой строк  
кода, в духе `is-odd` или `is-positive`.

## 4.5 HTTP запросы из Node.js

* `node-fetch` - API fetch для Node.js
* `isomorphic-unfetch` - fetch для изоморфных приложений
* `axios` - Более высокоуровневая библиотека, также изоморфна.

Для высоко нагруженных приложений со сложными требованиями может  
понадобится нативный API.

## 5. Асинхронный код

`.then(callback)` - в promise-ах можно ставить несколько раз.

`await` подвешивает функцию, что может повлиять на её скорость выполнения.

Преимущества Bluebird:

* Лучшая работа с ошибками программиста
* Расширенное API

```typescript
const Promise = require('bluebird');

// Аналог util.promisify
const readFile = Promise.promisify(require('fs').readFile);

// Аналог util.promisify для всего модуля или для методов класса
const fs = Promise.promisifyAll(require('fs'));
```

`Bluebird` - удобно, но с Node.js >= 12 не обязательно.

## 6.1 Введение

`app.get(path, callback);` - добавление пути

`res.send(content)` - Отправка ответа

`res.json(content)` - отправка JSON ответа

## 6.3 middleware

Middleware библиотеки для Express.js

* `cookie-parser` - библиотека для работы с cookie
* `compression` - библиотека для сжатия трафика
* `cors` - библиотека CORS
* `csurf` - CSRF библиотека
* `helmet` - библиотека для установки заголовков увеличивающих безопасность
* `morgan` - автоматическое логирование запросов

Помимо этого можно установить специальный middleware с 4 параметрами.

```typescript
app.use((err, req, res, next) => {
  console.error(err);
  res.status(400).send(err.message);
});
```

## 6.4 REST API

Для разделения кода можно использовать объект router.  
`const router = express.Router();`

## 6.5 Загрузка файлов

`npm i multer` - обработчик загрузки

Также необходимо ставить ограничение на размер файла и на допустимые типы  
файлов.

`npm i multer-s3` - для загрузки на AWS S3 (нагружает Node.js сервер, чего  
можно избежать с помощью других методов. Например как
[здесь](https://devcenter.heroku.com/articles/s3-upload-node))

`npm i passport` - для аутентификации с помощью сторонних сервисов

## 7.5 PostgreSQL: миграции

`npx knex migrate:latest` - применить новые миграции
`npx knex migrate:rollback` - откатить последние миграции

## 7.7 Другие актуальные реляционные БД

Другие БД, которые работают с Knex.js:

* MySQL
* MariaDB
* SQLite
* SQL Server (Microsoft)
* Oracle Database

Облачные сервисы поддерживающие хотя бы одну из это БД:

* Amazon RDS
* Google Cloud Services
* Microsoft Azure

## 8.4 Язык запросов MongoDB

```typescript
const db = client.db("database_name");

// получение коллекции (аналога таблицы)
const collection = db.collection("users"); 

collection.find(searchData);

collection.findOne(searchData);
// Вместо id должен быть передан объект - результат функции
collection.findOne({ _id: ObjectId(id) });

// Возвращает объект с различными данными результата
collection.insertOne(data);

collection.updateOne(searchData, updateObject);

collection.deleteOne(searchData);
```

## 8.5 MongoDB: миграции

Так как в большинстве NoSQL (в том числе и в MongoDB) баз данных нет схемы,  
основная задача миграции - это миграция данных. Например перенос документов  
из одной коллекции в другую, переименование аттрибутов и иногда более сложные  
трансформации.

`npm i migrate-mongo` - пакет для миграций MongoDB

## 8.6 Авторизация

При работе с MongoDB можно использовать camel case вместо snake case.

## 8.7 Другие актуальные нереляционные БД

* Redis - key-value БД
* Couchbase - мульти-модельная БД
* Riak - распределенный key-value store
* Elasticsearch - создан для полнотекстового поиска, поддержка JSON-документов
* Apache Cassandra - wide-column store
* Amazon DynamoDB - key-value и документная БД

## 9. CLI программы

`npm i yargs` - парсер аргументов командной строки
`npm i minimist` - более простая альтернатива `yargs`

## 9.2 Ввод данных из консоли

Базовый вариант для интерактивного диалога - это встроенный модуль `readline`
Скрыть пароль с помощью `readline` нельзя.

```typescript
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});
```

`npm i inquirer` - пакет для более продвинутой функциональности

## 9.3 Продвинутая FS

`npm i rimraf` - аналог `rm -rf` для Node.js

`npm i mkdirp` - создание директорий с родителями

Эти пакеты также можно использовать в npm скриптах,  
например `npx rimraf some_dir`.

`npm i fs-extra` - набор утилит для работ с файлами

`npm i node-walk` - обход дерева файловой системы  
`npm i klaw` - более сложная альтернатива

`npm i glob` - поиск файлов с помощью паттерна

## 9.4 Продвинутый вывод

Для вывода можно использовать символы Unicode (например значок папки для  
вывода вместе с информацией о директории)

`npm i chalk` - пакет для цветного вывода  
`npm i kleur` - альтернатива

`npm i cli-table` - вывод таблицы

`npm i draftlog` - перезапись вывода

`npm i blessed` - пакет для создания высокоуровневого интерфейса в терминале  
(виджеты и т.д.)

## 10.2 Node.js: жизненный цикл процесса

Node завершает работу только если нет активных таймеров или асинхронных  
процессов.

Особые ситуации:

* логическая ошибка
* Uncaught Exception
* Unhandled Rejection
* system signal

Код выхода 0 - успех, остальные числа - ошибка.

События:

* `exit` - процесс уже вышел из event-loop
* `beforeExit`
* `uncaughtException`
* `unhandledRejection` - пока что предупреждения, но станут ошибками
* `SIGINT` - после обработки нужно вручную завершать приложение
* `SIGTERM` - после обработки нужно вручную завершать приложение

При отлове не пойманной ошибки рекомендуется вывести нужную информацию и  
завершить процесс, т.к. при таких ситуациях скорее всего уже нельзя продолжить  
работу или уже поздно пытаться что-то нормализовать.

In both of these cases, you should do something counter-intuitive and let  
your program crash! Please don't try to be clever and introduce some complex  
logic trying to prevent a process restart. Doing so will almost always  
leave your application in a bad state, whether that's having a memory leak  
or leaving sockets hanging. It's simpler to let it crash, start a new  
process from scratch, and continue receiving more requests.

Whenever I work on a new Node.js project, I use the same function to  
ensure that my crashes are logged and my recoveries are guaranteed.  
It looks something like this:

```TypeScript
function terminate (server, options = { coredump: false, timeout: 500 }) {
  // Exit function
  const exit = code => {
    options.coredump ? process.abort() : process.exit(code)
  }

  return (code, reason) => (err, promise) => {
    if (err && err instanceof Error) {
    // Log error information, use a proper logging library here :)
    console.log(err.message, err.stack)
    }

    // Attempt a graceful shutdown
    server.close(exit)
    setTimeout(exit, options.timeout).unref()
  }
}

module.exports = terminate
```

## 10.3 Node.js event loop, очереди задач

Фазы event loop:

* `timers` - Executes callbacks scheduled by `setTimeout()` and
  `setInterval()`.
* `pending` - Executes I/O callbacks deferred to the next loop iteration.
* `idle` - prepare: Internal phase for system housekeeping (not
  directly accessible).
* `poll` - Retrieves new I/O events; executes related callbacks.
* `check` - Executes `setImmediate()` callbacks.
* `close` - Executes close event callbacks.
* `nextTickQueue` - Processes `process.nextTick()` callbacks.
* `microtaskQueue` - Processes micro tasks (e.g., Promise resolutions).

`nextTickQueue` и `microtaskQueue` выполняются каждый раз перед остальными  
фазами

С каждой фазой связана своя очередь.

In a server, you should not use the following synchronous APIs  
from these modules:

Encryption:

* crypto`.randomBytes` (synchronous version)
* crypto`.randomFillSync`
* crypto`.pbkdf2Sync`
* You should also be careful about providing large input to the  
  encryption and decryption routines.

Compression:

* `zlib.inflateSync`
* `zlib.deflateSync`

File system:

* Do not use the synchronous file system APIs. For example, if the  
  file you access is in a distributed file system like NFS, access times  
  can vary widely.

Child process:

* `child_process.spawnSync`
* `child_process.execSync`
* `child_process.execFileSync`

## 10.4 Работа с Buffer

Buffer - специальный тип для бинарных данных. Это массив 8 битных чисел  
без знака.  
Буфер имеет фиксированный размер заданный при создании, который не может быть  
изменён.

Из буфера можно выделить подмассив. (`buffer.slice(start, end)`)

Буферы и подбуферы можно перевести в строку.  
`subBuf.toString("utf8");`  
Также есть кодировки `hex` и `base64`.

## 10.5 Генераторы и итераторы

В JavaScript итератор - это объект, который предоставляет метод next(),  
возвращающий следующий элемент последовательности.

Генераторы – новый вид функций в современном JavaScript. Они отличаются  
от обычных тем, что могут приостанавливать своё выполнение, возвращать  
промежуточный результат и далее возобновлять его позже, в произвольный  
момент времени.

Генераторы можно компоновать.

## 10.6 Краткое введение в streams

Типы Streams:

* Readable
* Writable
* Duplex - можно и читать и писать
* Transform - in Node.js are a type of stream that can be used to  
  manipulate or transform data as it flows through the stream pipeline.

Каждый стрим как правило ведёт внутренний буфер данных, в котором  
накапливается очередной chunk. Размер этого буфера можно контролировать с  
помощью параметра `highWaterMark`.

На стрим можно подписаться с помощью callback или события, либо перенаправить  
стрим на другой стрим. Также возможно чтение с помощью `async` итератора.

Стрим может не успевать переваривать информацию, это называется `Backpressure`.

Запись стрима происходит двумя способами:

1. С помощью метода write
2. Перенаправление потока

## 10.7 HTTPS

Генерация самоподписанного сертификата

```bash
openssl genrsa -out key.pem
openssl req -new -key key.pem -out csr.pem
openssl x509 -req -days 9999 -in csr.pem -signkey key.pem -out cert.pem
rm csr.pem 
```

Не обязательно реализовывать HTTPS в коде Node.js. HTTPS может быть  
реализован с помощью обратного прокси с `nginx` например.

Существуют однако свои нюансы работы с ним, например необходима  
специальная настройка, чтобы увидеть IP пользователя:

`app.set('trust proxy', true);`
`req.ip || req.connection.remoteAddress`

## 11.2 WebSockets

Для перехода с HTTP на WebSockets используются специальные заголовки.

Основной способ работы с сокетами - это события:

* `open`
* `message`
* `error`
* `close`

Broadcast - сообщение от сервера всем клиентам (или группе).

## 11.3 Клиент-серверное взаимодействие

* [Простая библиотека для WebSockets](https://github.com/websockets/ws)
* [Альтернатива](https://github.com/uNetworking/uWebSockets.js)

## 11.4 WebSockets в браузере

`isomorphic-ws` - пакет для работы с WebSockets как в браузере, так и в  
Node.js.

## 11.5 Авторизация и аутентификация

Варианты авторизации с WebSockets:

* аутентификация в момент upgrade-запроса
* решение на уровне сокет-сообщений

## 12.1 Тестирование

Типы тестов:

* Unit Testing
* Component Testing
* Smoke Testing
* Integration Testing
* Regression Testing
* Sanity Testing
* System Testing
* User Acceptance Testing

`npm i debug` - пакет для вывода на основании переменной окружения `DEBUG`

## 12.3 npm scripts

Стандартные скрипты можно использовать без `run`:

* `npm test`
* `npm start`
* `npm stop`
* `npm restart` (запускает `stop` `restart` и `start`)

Конвенционные скрипты:

* `npm run build`
* `npm run dev`

Можно указать дополнительные действия для скрипта добавляя приставку `pre`  
или `post`, например `prebuild` или `postbuild`.

## 12.4 Полезные инструменты

* `nodemon` - перезапуск программы
* `yarn` - альтернатива npm
* `rome` - Rome is designed to replace Babel, ESLint, webpack, Prettier,  
  Jest, and others.
