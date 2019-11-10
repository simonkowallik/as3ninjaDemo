# as3ninjaDemo


Huh, what's this?

Check out: [/simonkowallik/as3ninja](https://github.com/simonkowallik/as3ninja)

Documentation at: [as3ninja.readthedocs.io](https://as3ninja.readthedocs.io)


## explore examples/

```shell
.
├── README.md  <--- You are here!
├── examples
│   ├── simple
│   │   ├── README.md
│   │   ├── declaration.json
│   │   ├── declaration_with_overlay.json
│   │   ├── http_path_header.iRule
│   │   ├── ninja.yaml
│   │   ├── overlay.json
│   │   ├── sorry_page.iRule
│   │   └── template.j2
│   └── yaml_datatypes
│       ├── README.md
│       ├── config.yaml
│       ├── output.json
│       └── template.j2
└── ninja.yaml
```



## run the as3ninja docker container

```shell
docker run -it --rm -p 8000:8000 simonkowallik/as3ninja:latest
```

## test the API UI

Navigate to: [http://localhost:8000/api/docs](http://localhost:8000/api/docs)
 -> `/api/declaration/transform/git`

Use this request body:

```json
{
  "repository": "https://github.com/simonkowallik/as3ninjaDemo"
}
```

## use curl

```shell
curl -sX POST http://localhost:8000/api/declaration/transform/git \
  -H "Content-Type: application/json" -d \
  '{"repository":"https://github.com/simonkowallik/as3ninjaDemo"}'
```

optionally with `jq`:

```shell
curl -sX POST http://localhost:8000/api/declaration/transform/git \
  -H "Content-Type: application/json" -d \
  '{"repository":"https://github.com/simonkowallik/as3ninjaDemo"}' \
  | jq .
```

validate the generated declaration:
```shell
curl -sX POST http://localhost:8000/api/declaration/transform/git \
  -H "Content-Type: application/json" -d \
  '{"repository":"https://github.com/simonkowallik/as3ninjaDemo"}' \
    | curl -sX POST http://localhost:8000/api/schema/validate \
    -H "Content-Type: application/json" -d@-
```

demonstrate a validation failure by using an older schema version:
```shell
curl -sX POST http://localhost:8000/api/declaration/transform/git \
  -H "Content-Type: application/json" -d \
  '{"repository":"https://github.com/simonkowallik/as3ninjaDemo"}' \
    | curl -sX POST "http://localhost:8000/api/schema/validate?version=3.8.1" \
    -H "Content-Type: application/json" -d@-
```

## or httpie

```shell
http http://localhost:8000/api/declaration/transform/git \
  repository=https://github.com/simonkowallik/as3ninjaDemo
```

validate the generated declaration:
```shell
http http://localhost:8000/api/declaration/transform/git \
  repository=https://github.com/simonkowallik/as3ninjaDemo \
  | http http://localhost:8000/api/schema/validate
```

demonstrate a validation failure by using an older schema version:
```shell
http http://localhost:8000/api/declaration/transform/git \
  repository=https://github.com/simonkowallik/as3ninjaDemo \
  | http "http://localhost:8000/api/schema/validate?version=3.8.1"
```


## how does it work?

POSTing the below JSON body to `/api/declaration/transform/git` instructs AS3 Ninja to fetch the repo and create an AS3 declaration based on the default configuration files in the repository. AS3 Ninja checks for the following files `ninja.json`, [`ninja.yaml`](ninja.yaml), `ninja.yml` and uses the first file it finds.

```json
{
  "repository": "https://github.com/simonkowallik/as3ninjaDemo"
}
```

The response is the AS3 declaration, which can be validated against the AS3 schema. The latest available schema (in the docker container) is used by default, a specific version can be specified using the `version` query parameter, as demonstrated above.
