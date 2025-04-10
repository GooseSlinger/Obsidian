### **1xx: Информационные**

Эти коды указывают, что сервер принял запрос и продолжает его обрабатывать.

- **100 Continue** : Сервер получил начальную часть запроса и готов принять остальное.
- **101 Switching Protocols** : Сервер согласен изменить протокол, как было предложено клиентом.
- **102 Processing** : Запрос был принят, но его обработка еще не завершена (часто используется в связке с WebDAV).
- **103 Early Hints** : Предварительная информация о ресурсах, которые могут потребоваться для загрузки страницы.

### **2xx: Успешные**

Эти коды показывают, что запрос успешно выполнен.

- **200 OK** : Запрос успешен, и данные отправлены в теле ответа.
- **201 Created** : Запрос успешно выполнен, и создана новая ресурс.
- **202 Accepted** : Запрос принят, но его обработка еще не завершена.
- **203 Non-Authoritative Information** : Запрос успешен, но ответ может быть модифицирован промежуточным сервером.
- **204 No Content** : Запрос успешен, но нет содержимого для передачи.
- **205 Reset Content** : Запрос успешен, клиент должен очистить текущий документ.
- **206 Partial Content** : Частичный контент успешно передан (используется при скачивании файлов по частям).
- **207 Multi-Status** : Множественные состояния (часто используется в WebDAV).
- **208 Already Reported** : Используется в WebDAV для предотвращения дублирования данных.
- **226 IM Used** : Сервер успешно обработал запрос, используя указанное представление.

### **3xx: Перенаправления**

Эти коды указывают, что клиент должен выполнить дополнительные действия для завершения запроса.

- **300 Multiple Choices** : Несколько вариантов ресурса доступны, клиент должен выбрать один.
- **301 Moved Permanently** : Ресурс был перемещен на новый URL.
- **302 Found** : Временное перенаправление на другой URL.
- **303 See Other** : Клиент должен сделать GET-запрос к другому URL.
- **304 Not Modified** : Ресурс не изменился с момента последнего запроса.
- **305 Use Proxy** : Доступ к ресурсу возможен только через прокси.
- **306 Switch Proxy** : Код больше не используется.
- **307 Temporary Redirect** : Временное перенаправление без изменения метода запроса.
- **308 Permanent Redirect** : Постоянное перенаправление без изменения метода запроса.

### **4xx: Ошибки клиента**

Эти коды указывают на ошибки, вызванные некорректными запросами клиента.

- **400 Bad Request** : Неверный синтаксис или недопустимый запрос.
- **401 Unauthorized** : Требуется аутентификация для доступа к ресурсу.
- **402 Payment Required** : Зарезервирован для будущего использования.
- **403 Forbidden** : Доступ запрещен, даже если пользователь авторизован.
- **404 Not Found** : Запрошенный ресурс не найден.
- **405 Method Not Allowed** : Метод запроса (GET, POST и т.д.) запрещен для данного ресурса.
- **406 Not Acceptable** : Сервер не может предоставить данные в формате, понятном клиенту.
- **407 Proxy Authentication Required** : Требуется аутентификация через прокси.
- **408 Request Timeout** : Сервер ждал слишком долго ответа от клиента.
- **409 Conflict** : Конфликт при попытке выполнить запрос.
- **410 Gone** : Ресурс больше не существует и не будет восстановлен.
- **411 Length Required** : Необходим заголовок `Content-Length`.
- **412 Precondition Failed** : Предусловие запроса не выполнено.
- **413 Payload Too Large** : Размер передаваемых данных превышает допустимый лимит.
- **414 URI Too Long** : URI слишком длинный.
- **415 Unsupported Media Type** : Формат передаваемых данных не поддерживается.
- **416 Range Not Satisfiable** : Запрос диапазона данных невозможен.
- **417 Expectation Failed** : Ожидание клиента не может быть выполнено.
- **418 I'm a teapot** : Юмористический код, часто используется для тестирования.
- **421 Misdirected Request** : Запрос был направлен не к тому серверу.
- **422 Unprocessable Entity** : Запрос понятен, но не может быть выполнен из-за семантической ошибки.
- **423 Locked** : Ресурс заблокирован.
- **424 Failed Dependency** : Ошибка зависимости.
- **425 Too Early** : Сервер не хочет рисковать обработкой запроса до завершения проверки.
- **426 Upgrade Required** : Необходимо обновить протокол.
- **428 Precondition Required** : Необходимо использовать предусловия.
- **429 Too Many Requests** : Превышено количество запросов.
- **431 Request Header Fields Too Large** : Заголовки запроса слишком большие.
- **451 Unavailable For Legal Reasons** : Доступ ограничен по юридическим причинам.

### **5xx: Ошибки сервера**

Эти коды указывают на ошибки, возникшие на стороне сервера.

- **500 Internal Server Error** : Произошла внутренняя ошибка сервера.
- **501 Not Implemented** : Сервер не поддерживает необходимую функциональность.
- **502 Bad Gateway** : Сервер получил недействительный ответ от upstream-сервера.
- **503 Service Unavailable** : Сервис временно недоступен.
- **504 Gateway Timeout** : Upstream-сервер не ответил вовремя.
- **505 HTTP Version Not Supported** : Версия HTTP не поддерживается сервером.
- **506 Variant Also Negotiates** : Конфликт при выборе варианта ресурса.
- **507 Insufficient Storage** : Недостаточно места для хранения данных.
- **508 Loop Detected** : Обнаружен бесконечный цикл.
- **510 Not Extended** : Дополнительные условия не удовлетворены.
- **511 Network Authentication Required** : Требуется сетевая аутентификация.