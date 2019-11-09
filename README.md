# as3ninjaDemo


Huh, what's this?

Check out: [https://github.com/simonkowallik/as3ninja](/simonkowallik/as3ninja)

Documentation at: [https://as3ninja.readthedocs.io](as3ninja.readthedocs.io)


## explore examples/

```shell
.
├── README.md
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


## or httpie

```shell
http http://localhost:8000/api/declaration/transform/git \
  repository=https://github.com/simonkowallik/as3ninjaDemo
```
