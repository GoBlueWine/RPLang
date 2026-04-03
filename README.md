# RPLang

RPLang is a model description language for Go projects.

It lets you describe your models once and generate the repetitive layers around them: transport code, storage helpers, validation, metadata, and other runtime-specific glue.

RPLang is part of the broader GoBlueWine ecosystem, where the goal is not just to ship isolated packages, but to make Go more comfortable for building complete products: services, websites, internal tools, and full applications.

## Why RPLang

In many Go projects, the model is written once, but the same information is repeated again and again:

- API contracts
- gRPC and transport glue
- SQL mapping and queries
- Redis serialization
- validation rules
- metadata and helper code

RPLang exists to keep the model as the source of truth and generate the surrounding code from that model instead of rewriting it by hand.

## What It Looks Like

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

From a schema like this, RPLang can generate useful Go code around the model instead of turning the model into a transport-specific object.

## What It Generates

RPLang is designed to generate practical code, not toy demos.

Depending on the attrs you attach, the language can drive generation for:

- plain Go model packages
- gRPC transport layers
- SQL schema and CRUD helpers
- Redis key and hash helpers
- validation and sensitive-field hashing
- standard model metadata
- custom attrs written through the SDK

One important example is gRPC: the current design is service-first, so the generated code wraps a normal Go service interface instead of pretending the server is a single in-memory model instance.

## What Kind of Project This Supports

RPLang is aimed at projects that want a stronger development flow around models.

That includes:

- backend services
- websites and web applications
- internal business systems
- modular Go applications
- higher-level ecosystems built from reusable modules, including ideas such as iGo

The larger vision is to make Go development more complete: define the data model once, then build the rest of the system around it with reusable tooling.

## Current Direction

RPLang is currently focused on Go as the main target language.

The language is being shaped around:

- model-first design
- explicit attrs instead of hidden magic
- generated code that still looks like normal Go
- modular extension through runtime attrs and SDK packages

## Status

RPLang is in active development and still evolving.

The language, generators, and surrounding tooling are being built as part of the GoBlueWine ecosystem. Expect rapid iteration, especially around developer experience, generated APIs, and the attr SDK.

## Learn More

- [Organization: GoBlueWine](https://github.com/GoBlueWine)
- [BlueWine framework](https://github.com/GoBlueWine/BlueWine)
- [RPLang wiki](https://github.com/GoBlueWine/RPLang/wiki)

## License

Apache 2.0.
