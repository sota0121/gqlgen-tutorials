# How to develop a graphQL server with gqlgen

## References

- https://gqlgen.com/
- https://github.com/99designs/gqlgen
- https://www.asobou.co.jp/blog/web/gqlgen

## Overview

after creating your project, you need to do the following steps:

1. Install gqlgen

```bash
go get github.com/99designs/gqlgen
```

2. Generate the code using `gqlgen init`

```bash
go run github.com/99designs/gqlgen init
```

3. Write schema in `graph/schema.graphqls`
4. Implement the Resolver in `graph/*.resolvers.go`
5. Write DI components in `graph/resolver.go`
6. Run server using `go run server.go`


---

## Copied from Note

1. install `gqlgen` to project
2. execute `gqlgen init` command
    1. like this `go run [github.com/99designs/gqlgen](http://github.com/99designs/gqlgen) init`
    2. then, these files are generated
        1. `gqlgen.yml`
        2. `graph/`
            1. `generated/generated.go`
            2. `model/models_gen.go`
            3. `resolver.go`
            4. `schema.graphqls`
            5. `schema.resolvers.go`
        3. `server.go`
3. Write schema into `schema.graphqls` file
4. Implement resolver on `schema.resolvers.go`
    1. ==========================================
    2. resolver は、スキーマとデータソースを結びつける役割がある
    3. `gqlgen generate` コマンドにより、 `graph/schema.graphqls` がモデルの `graph/model/*` と比較され、可能な限りモデルに直接バインドされる。
    4. `init` が実行された時、内部で `generate` も行われている
    5. ==========================================
5. Implement `resolver.go`
    1. `resolver.go` > `Resolver` type には、DIパターンでさまざまなモデル？グラフ？サービス？を追加していく。
    2. `server.go` でこの Resolver は初期化される。
        1. 自動生成されたコードだとこんな感じ
        2. `srv := handler.NewDefaultServer(graph.NewExecutableSchema(graph.Config{Resolvers: &graph.Resolver{}}))`
6. Run server using `go run server.go` command
