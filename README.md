# API Guidelines

## Введение

Программная архитектура цифровой трансформации Сибур построена вокруг слабо связанных микросервисов, которые предоставляют 
функциональность с помощью API. Проектные команды, владеют, разрабатывают, развертывают и поддерживают эти микросервисы.
API микросервисов максимально полно отражают свою функциональность. Разработка качественных, долговечных и производительных API, 
является приоритетной задачей.

API First является одним из наших ключевых принципов проектирования. Разработка микросервисов начинается с проектирования API.
Что в идеале включает в себя обширную обратную связь от клиентов API для получения высококачественных API-интерфейсов. 
API First включает в себя набор стандартов описанных в данном документе и поддерживает культуру коллегиального обзора. 
Мы призываем наши команды следовать им, чтобы гарантировать, что наши API:
- можно легко и быстро понять
- просты в использовании
- следуют единому стилю
- совместимы с API других команд и нашей глобальной архитектурой

В идеале все API-интерфейсы должны выглядеть так, как будто их создал один и тот же автор.

Цель документа "API Guidelines" в том, чтобы определить стандарты по разработке API, 
чтобы придать всем API "единый стиль и общий дух".

## Соглашения используемые в документе

Ключевые слова используемые для указания уровня требований должны быть интерпретированы как в [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt):
- ДОЛЖЕН, ТРЕБУЕТСЯ, ОБЯЗАН ([RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) пункт 1)
- НЕ ДОЛЖЕН, НЕ ТРЕБУЕТСЯ, НЕ ОБЯЗАН ([RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) пункт 2)
- РЕКОМЕНДУЕТСЯ ([RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) пункт 3)
- НЕ РЕКОМЕНДУЕТСЯ  ([RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) пункт 4)
- МОЖЕТ, ОПЦИОНАЛЬНО ([RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) пункт 5)

Данные слова используются напрямую в тексте, в любом регистре и допускаются различные окончания и другие способы словообразования.
Всё это не изменяет интерпретацию ключевых слов.

## Ограничения и требования накладываемые данным документом

Владельцами данного документа является представители различных проектных команд,
входящих в команду [API Guild](https://github.com/orgs/sibur/teams/api-guild),
его изменение проводится на основе голосования всеми участниками [API Guild](https://github.com/orgs/sibur/teams/api-guild). 
Решения о внесении изменений вносятся только с полного согласия с изменениями 
всех участников команды [API Guild](https://github.com/orgs/sibur/teams/api-guild),
то есть либо с изменением согласны все, либо оно отвергается.
 
Команды обязаны соблюдать данный документ во время разработки API. Любая команда может внести свой вклад 
в разработку руководящих принципов создав Pull request.
Рассмотрение, принятие или отклонение изменений внесенных в Pull request 
находится в компетенции команды [API Guild](https://github.com/orgs/sibur/teams/api-guild).

Эти руководящие принципы в какой-то мере будут изменяться и развиваться в процессе нашей работы, 
но команды могут уверенно следовать им и доверять им.

В случае когда API Guidelines изменились, следующие правила применяются:

- существующие API не обязаны изменяться, но это рекомендуется
- новые версии существующих API должны следовать новым правилам, после обновления major версии
- все API микросервисов, которые являются инфраструктурными, такие как аутентификация и авторизация, уведомления и подобные
 должны следовать новым правилам всегда
- Новые API обязаны следовать новым правилам

При разработке API, любого микросервиса, разработчик обязан руководствоваться данными методическими
указаниями.

В качестве примера API, которое максимально следует данным рекомендациям в папке example содержится API, 
при разработке API с нуля рекомендуется брать данный шаблон за основу.

## Основопологающие принципы

- API as a product: API должно разрабатываться так, как если бы оно могло бы быть открытым неопределенному
кругу лиц и могло бы решать их проблемы как целостный продукт.
- API first:  при разработке любого  микросервиса, с которым подразумевается внешнее взаимодействие,
разработка API является первичной и приоритетной.
- GRPC API Only: для описания API любого микросервиса должен использоваться 
только [GRPC](https://grpc.io/) интерфейс взаимодействия с ним, 
само API описывается с помощью [protocol-buffers proto3](https://developers.google.com/protocol-buffers/docs/proto3).
- One API for one microservice: любой  микросервис, должен иметь одно и только одно GRPC API,
и каждое API может быть реализовано только одним микросервисом.
- REST, We Can: в случаях, когда необходим REST API, требуется использовать [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway),
а не реализовывать REST интерфейс микросервиса отдельно. Если REST API реализовано, должно быть сгенерировано его Swagger-описание.
- Simple Client Metadata: любой клиент API, рекомендуется использовать только токен аутентификации в метадате запроса.
 Другие данные в метадате от клиента не могут ожидаться и использоваться, но это не рекомендуется.  
- Server Metadata: метаданные в ответах сервера могут и должны использоваться в случаях, 
когда это оправдано (указании версий ответа, реализации сквозных id запросов, и т.п.).
- Self-documenting API:  содержимое proto файлов должно быть прокомментировано до такой степени, чтобы было
полностью понятно, без дополнительной документации, стороннему клиенту.

## Репозиторий API

- Для хранения исходных кодов API должен использоваться VCS GIT
- Все репозитории с API должны храниться в [gitlab](https://gitlab.com/sibur/api) 
- Любой клиент может использовать любое API в [gitlab](https://gitlab.com/sibur/api)
- Способ, которым клиент подключает API, может быть любой удобный клиенту
- Рекомендуется подключать API через git submodules и генерировать client API непосредственно во время сборки проекта
- Любой клиент должен следить за обновлениями репозитория API и должен самостоятельно обновлять клиентское API
- При обновлении версии API пользователи могут уведомляются через автоматизированный канал
- В названии должен использовать только нижний регистр, слова должны разделяться с помощью - (hyphen)
- Имя репозитория API должно быть уникальным в рамках [gitlab](https://gitlab.com/sibur/api) 
- В имени репозитория не должны использоваться любые зарезервированные слова для любых языков
программирования
- В имени репозитория не должны использоваться какие угодно префиксы или постфиксы, для отражения каких
угодно особенностей API, в.т.ч. private, public, external, internal, stable, dev, production и.т.п.
- Любой репозиторий с API в корне должен содержать файлы README.md, DOCUMENTATION.md CHANGELOG.md, .gitignore и папку proto
- Папка proto должна содержать только файлы *.proto
- Папка proto не должна содержать вложенные папки

## Версионирование API

- API версионируется с помощью [семантического версионирования](https://semver.org/lang/ru/)
- До начала промышленной эксплуатации API версионируется по semver как нестабильный API (начиная с 0.0.1). 
В этом случае обратно не совместимые изменения, не ведут к изменению major версии.
- После начала эксплуатации API получает версию 1.0.0
- Изменения в API ниже major-версии не могут быть несовместимыми.
- Версионирование реализуется с помощью тегирования средствами Git master ветки в [gitlab](https://gitlab.com/sibur/api). 
- Не должно использоваться версионирование в URI
- Ответ сервера может содержать версию API в метаданных
- Все изменения в API после версии 1.0.0 отражаются в CHANGELOG.md

## README.md

- должен содержать информацию на каком хосте и порте находится сервер реализующий API
- должен содержать описание предметной области и то какую задачу решает API
- должен содержать глоссарий с определением всех терминов предметной области, которые используются в API, не должны использоваться сокращения
- должен содержать ссылку на DOCUMENTATION.md
- должен содержать ссылку на CHANGELOG.md

## DOCUMENTATION.md

- должен содержать автоматически сгенерированную документацию на основе файлов в папке proto
- документация должна генерироваться с помощью [protoc-gen-doc](https://github.com/pseudomuto/protoc-gen-doc)

## CHANGELOG.md

- должен содержать список изменений API от версии к версии в обратном хронологическом порядке (самое новое - в начале)
- должен обновляться одновременно с выпуском каждой новой версии API начиная с 1.0.0

## Authentication 

TODO

## Code Style rules

- API должно следовать следующему [code style](https://developers.google.com/protocol-buffers/docs/style)
- для отступов должна использоваться табуляция, 1 табуляция равна 4 пробелам
- все rpc отделяются друг от друга одной строкой
- для комментирования должна использоваться // нотация
- все комментарии пишутся перед комментируемой сущностью, на отдельной строке
- все message, enum и service отделяются друг от друга одной строкой
- все параметры внутри message и enum отделяются друг от друга отступом в одну строку
- service должны именоваться в UpperCamelCase
- message должны именоваться в UpperCamelCase
- rpc должны именоваться в lowerCamelCase
- param должны именоваться в lower_snake_case
- enum должны именоваться в UpperCamelCase
- enum param должны именоваться в SCREAMING_SNAKE_CASE

## *.proto file rules
- все *.proto файлы должны распологаться в папке proto
- *.proto файл должен содержать только одну сущность предметной области
- *.proto файл должен содержать сервис для манипуляций с сущностью предметной области, которая распологается в этом файле
- *.proto файл должен содержать все Request и Response для сервиса, который находится в этом файле
- *.proto файл должен именоваться в lowerCamelCase
- название *.proto файла должно повторять название сущности предметной области, которая находится в нем. 
- содержимое *.proto файла должно быть написано на английском, за исключением комментариев
- комментарии должны быть написаны либо на русском либо на английском
- На первой строчке *.proto должно содержаться указание syntax= 'proto3';
- на второй строчке должен быть указан package, после него отступ в одну строчку
- В имени пакета должен использоваться UpperCamelCase
- Для именования пакета используется следующий шаблон: Api.Sibur.{{RepositoryName}}.{{FileNameWithoutExtension}}
- разрешено использовать import для импортирования файла structures.proto
- разрешено использовать import для импортирования "google/api/annotations.proto";
- рекомендется не использовать другие import кроме structures.proto и "google/api/annotations.proto"
- при импортировании файла structures.proto должен указываться относительный путь начиная с корневой папки репозитория
- остальную часть файла должны занимать описание непосредственно API
- первый message в *.proto файле должен быть идентичный имени файла message в UpperCamelCase, который является сущностью
пердметной области
- после message который является сущностью предметной области, должен идти service для манипуляций с ним.
- после service должны идти все request и response для сервиса, в хронологическом порядке объявления rpc в сервисе

## Привила сущности предметной области
- под моделью понимается message, который является сущностью предметной области
- название модели должно быть существительным, однозначным и отражать сущность предметной области
- название модели не должно содержать зарезервированные слова для языков программирования
- название модели не должно содержать какие угодно префиксы или постфиксы, для отражения каких
угодно особенностей, в.т.ч. private, public, external, internal, stable, dev, production, test, entity и.т.п.
- модель должна содержать внутри себя  Id
- первое поле модели должен быть id, в данном параметре задается уникальный ключ этой модели
- Модели которые содержат связи с другими моделями должны связываться с ними через их id, 
а не содержать саму структуру модели с которой есть связь
- Параметр который является указанием на другую модель, должен именоваться по шаблону {{model_name}}_id
- Типы данных параметра id в модели и всех других местах, где id этой модели используется должны совпадать
- Модель может содержать message Data, в данном message хранятся параметры, 
которые нужно указывать при создании модели с помощью метода create,
 а также те, которые можно свободно изменять методом update
- Модель может содержать любое количество вложенных message для отражения любых особенностей своей структуры
 
## Service rules 
- Каждый service должен отвечать за одну и только одну модель предметной области
- Имя service формируется по шаблону {{ModelName}}Service
- Название rpc процедуры должно быть глаголом, отражающем суть выполняемой операции
- В названии rpc процедуры не должно дублироваться имя сервиса или его части
- Если сервис предполагает CRUD операции, то он должен содержать их с следующими именами: get, create, update, delete
- Все rpc кроме get, create, update, delete должны быть прокомментированы
- Все rpc на вход должны получать message с постфиксом Request
- Все rpc на выход должны получать message с постфиксом Response
- Каждая rpc должна содержать option для описания в REST
- Для описания REST методов используется google.api.http
- В качестве pattern должен использоваться только post или get
- путь в post указывается по следующему шаблону "/{{model}}/{{rpc}}", где model - имя модели предметной области, 
rpc - название rpc метода, UpperCamelCase и lowerCamelCase в названиях message и rpc заменяются на kebab-case
- В качестве body всегда указывается "*"
- Более сложные маппинги в рамках [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway) не должны использоваться
- Маппинги на путь в post параметров запроса не должны использоваться

## Request and Response rules
- Все Request должны именоваться по принципу {{RpcName}}Request
- Рекомендуется делать Request максимально простым для клиента
- Все Response должны именоваться по принципу {{RpcName}}Response
- Все Response должны содержать oneof параметр result, где внутри результат выполнения команды или Error
- Результат выполнения может быть в любым типом
- Параметр результата выполнения, команды должен называться data
- Рекомендуется в качестве результата выполнения команды возвращать изменяемую модель, там где это имеет смысл и возможно

## Структура Error
- У каждого Response должен быть объявлен message Error
- Внутри Error должен быть oneof type, внутри которого общая ошибка или спецефичная для этого ответа
- первым параметром внутри  oneof type идет common_error
- вторым параметром внутри  oneof type идет specific_error
- общие ошибки выносятся в файл structures.proto
- общие ошибки это Enum c именем CommonError
- общие ошибки нулевым элементом должны содержать INTERNAL_ERROR, которая говорит о наличии неучтенной ошибки сервера
- общие ошибки первым элементом должны содержать FORBIDDEN, которая говорит о запрете выполнения операции из-за отсутсвия соответвующих привелегий
- спецефичные ошибки должны быть объявлены внутри message Error и называться SpecificError
- структура SpecificError может быть любой
- рекомендуется использоваться следующий шаблон для описания ошибок:
```proto
    message Error {
        oneof type {
            Structures.CommonError common_error = 1;
            SpecificError specific_error = 2;
        }

        message SpecificError {
            Code code = 1;
            string message = 2;

            enum Code {
                STUDENT_NOT_FOUND = 0;
                REMOVAL_IMPOSSIBLE = 1;
            }
        }
    }

```

## Get Request и Response
- Запрос на получение списка модели предметной области должен быть сформирован по следующему шаблону:
```proto
    message GetRequest {
        repeated Scope scope = 2;
        uint64 offset = 3;
        uint64 length = 4;
    
        message Scope {
            oneof condition {
                ById by_id = 1;
                LikeFullName like_full_name = 2;
            }
    
            message ById {
                repeated uint32  some_model_id = 1;
            }
    
            message LikeFullName {
                string full_name = 1;
            }
        }
    }
```
- GetRequest может быть полностью пустым, если дополнительные условия поиска не предполагаются
- message Scope содержит список условий к которые применяются к выборке, 
список условией может расширяться в случае возникновения потребности в этом
- допускается использовать как несколько условий, так и не использовать их вовсе
- message Scope должен содержать oneof condition внутри которого указываются предикаты условий
- В качестве предикатов выступают message внутри Scope, каждое из которых указывает какое-то конкретное условие
- вторым параметром GetRequest должен идти uint64 offset, который указывает сдвиг выборки с начала до значения offset
- третьим параметром GetRequest должен идти uint64 length, который указывает максимальную длину выборки
- GetResponse должен быть сформирован по следующему шаблону: 
```proto
    message GetResponse {
        oneof result {
            Batch data = 1;
            Error error = 2;
        }

        message Batch {
            repeated SomeModel some_model = 1;
        }
        
        message Error {
            oneof type {
                Structures.CommonError common_error = 1;
                SpecificError specific_error = 2;
            }
    
            message SpecificError {
                Code code = 1;
                string message = 2;
    
                enum Code {
                    INVALID_SCOPE_CONDITION = 0;
                }
            }
        }
    }
```
- oneof result в GetResponse должен содержать Batch, внутри которого содержится результат выборки, 
а именно модели предметной области

## Structures file rules
- если в какой либо message приходится использовать в разных файлах, он должен быть вынесен в специальный файл structures.proto
- structures.proto единтсвенный файл для конкретного api, который можно импортировать в другие файлы *.proto 

## Работа с временем
- для передачи метки времени между сервером и клиентов, должно осуществляться только с  помощью следующего message:
```proto
    // Описания момента во времени. Определяется как количество милисекунд,
    // прошедших с полуночи (00:00:00 UTC) 1 января 1970 года (четверг)
    message Timestamp {
        // количество милисекунд, прошедших c 00:00:00 UTC 01.01.1970
        int64 milliseconds = 1;
    }
```
- данный message должен содержаться в structures.proto
- для передачи длительности по времени, должно осуществляться только с помощью следующего message:
```proto
    message Duration {
        // количество милисекунд
        int64 milliseconds = 1;
    }
```