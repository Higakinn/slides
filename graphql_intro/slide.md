---
marp: true
page_number: true
theme: gaia
paginate: true
class: lead
header: header
footer: footter
---
<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Serif&display=swap');

section {
    font-family: 'Noto Serif', serif;
}
</style>

<!-- headingDivider: 1 -->

<!-- #　見出しの前にスライドページを自動的に分割 -->

# GraphQL


# 目次
- GraphQLとは？
- Graphql Query, Mutationについて
- GraphQLの何がいいのか？


# GraphQL
- FaceBookがつくったAPI新規格
- 「クエリ言語」と「スキーマ言語」からなる 
  - クエリ言語・・APIリクエストするための言語
  - スキーマ言語・・API仕様を定義するための言語
  
- 特徴
  - クライアントがほしいデータだけを取得できる→パフォーマンスの向上
  - APIリクエストが直感的に書ける


# スキーマ例
APIの仕様を定義。書式は以下の通り
```
type [オブジェクト名] {
    [field名]: [型]
}
```
- オブジェクト名
  - Graphqlの予約語(`Query`,`Mutation`)や独自のオブジェクトを定義
- 型
    - `ID`, `Int`, `Float`, `String`,  `Boolean` に対応
# スキーマ例
userのidと名前を取得するためのスキーマ例
```graphql
type Query { "ユーザ情報を取得するためのクエリ"
    currentUsers: [User]!
}
type Mutation { "user"
    addUser(name: String): User!
}

type User { "idとnameのfieldを持つUser型"
    id: ID!
    name: String!
}
```

# クエリ例1
上記のスキーマに対して発行するクエリは

```graphql
query GetCurrentUsers {
    currentUsers {
        id
        name
    }
}
# curl -i -X POST -d "{ \"query\": \"query { GetCurrentUsers { currentUsers {id, name} }}\"}" [エンドポイント]
```
このように記述すると・・・

# クエリ結果1
このように結果が返ってくる。
```json
{
    "data": {
        "currentUser": {
            "id": "hogehoge",
            "name": "hugahuga"
        }
    }
}
```
# クエリ例2
例えば,nameだけほしい場合は以下のようなクエリにすればよい
```
query GetCurrentUser {
    currentUser {
        name
    }
}
```
# クエリ結果2
```json
{
    "data": {
        "currentUser": {
            "name": "hugahuga"
        }
    }
}
```
欲しいデータだけを取得することができるのがGraphQLのいいところ。
# クエリ例3
```
mutation {
    addUser(name: "test") {
        name
    }
}
```
# クエリ結果3
```json
{
    "data": {
        "addUser": {
            "name": "test"
        }
    }
}
```
# GraphQL Query, Mutaion 
- Query
  - データ取得したいときにつかう
- Mutation 
  - データ追加、更新、削除したいときに使う
- (subscription)
  - サーバーの通知につかう(今回は割愛)

<!-- ||REST|GraphQL|
:-:| :-:|:-:
| 取得 | GET| Query
| 登録 | POST | Mutation
| 更新 | UPDATE | Mutation
| 削除 | DELETE| Mutation -->

# GraphQLとRestfulAPIの違い
||REST|GraphQL|
:-:| :-:|:-:
| データ取得 | GET| Query
| データ登録 | POST | Mutation
| データ更新 | UPDATE | Mutation
| データ削除 | DELETE| Mutation
|エンドポイント|リソース毎に増える| 一つ


# まとめ
- GraphQL
    - webAPIの新規格で「クエリ言語」と「スキーマ言語」からなる
    - データの取得には**Query**,削除、更新、追加には**Mutation**を使う


