# RPLang

> Читать этот README на английском: [English version](README.md)

RPLang это язык описания моделей для Go-проектов.

Он позволяет один раз описать модель и генерировать вокруг неё повторяющиеся слои: transport code, storage helpers, validation, metadata и другой runtime-specific glue.

RPLang это часть более широкой экосистемы GoBlueWine. Её цель не просто выпускать отдельные пакеты, а сделать Go удобнее для сборки полноценных продуктов: сервисов, сайтов, внутренних инструментов и приложений.

## Зачем нужен RPLang

Во многих Go-проектах модель пишется один раз, но одна и та же информация потом дублируется снова и снова:

- API contracts
- gRPC и transport glue
- SQL mapping и queries
- Redis serialization
- validation rules
- metadata и helper code

RPLang нужен для того, чтобы модель оставалась источником правды, а окружающий код генерировался из неё, а не переписывался вручную.

## Как это выглядит

```rpl
target(lang: golang)

attrs (
    "petrov:grpc"
    "petrov:sql"
    "petrov:validate"
)

@comment("System user")
@grpc()
@sql(db: "postgres", table: "users")
model User {
    Id int
    {
        @sql(unique)
        @grpc.id()
    }

    Name string
    {
        @validate(min: 2, max: 32)
    }

    Email string
    {
        @validate(email)
    }
}
```

Из такой схемы RPLang может сгенерировать полезный Go-код вокруг модели, а не превращать саму модель в transport-specific object.

## Что он генерирует

RPLang задуман для генерации практичного кода, а не игрушечных демо.

В зависимости от attrs язык может управлять генерацией для:

- обычных Go model packages
- gRPC transport layers
- SQL schema и CRUD helpers
- Redis key и hash helpers
- validation и hashing чувствительных полей
- стандартной metadata для моделей
- custom attrs, написанных через SDK

Один из важных примеров это gRPC: текущий дизайн service-first, поэтому generated code оборачивает нормальный Go service interface вместо того, чтобы делать вид, будто сервер это один in-memory instance модели.

## Какие проекты это поддерживает

RPLang рассчитан на проекты, которым нужен более сильный поток разработки вокруг моделей.

Сюда входят:

- backend services
- сайты и web applications
- внутренние бизнес-системы
- модульные Go-приложения
- более высокоуровневые экосистемы из переиспользуемых модулей, включая идеи вроде iGo

Большая цель в том, чтобы сделать Go-разработку более цельной: описываешь модель один раз, а затем строишь вокруг неё остальную систему с помощью переиспользуемого tooling.

## Текущее направление

Сейчас RPLang в первую очередь сфокусирован на Go как на основном target language.

Язык формируется вокруг нескольких идей:

- model-first design
- явные attrs вместо скрытой магии
- generated code, который всё ещё выглядит как обычный Go
- модульное расширение через runtime attrs и SDK packages

## Статус

RPLang находится в активной разработке и продолжает меняться.

Сам язык, генераторы и окружающий tooling создаются как часть экосистемы GoBlueWine. Здесь стоит ожидать быстрой итерации, особенно вокруг developer experience, generated APIs и attr SDK.

## Узнать больше

- [Organization: GoBlueWine](https://github.com/GoBlueWine)
- [BlueWine framework](https://github.com/GoBlueWine/BlueWine)
- [RPLang wiki](https://github.com/GoBlueWine/RPLang/wiki)

## Лицензия

Apache 2.0.
