# FireQL
Query Google Firestore database using SQL syntax.

FireQL is a library built on top of the official [Google Firestore Client SDK](https://pkg.go.dev/cloud.google.com/go/firestore) that will allow running queries Cloud Firestore database using SQL syntax. Inspired by [Firebase FireSQL](https://firebaseopensource.com/projects/jsayol/firesql/).

## Usage

<!-- `FireQL` can be used Go library or interactive command-line tool. -->

### Go Library
An example of querying collections using SQL syntax:
```go
import (
    "github.com/pgollangi/fireql"
)

func main() {
    fql, err := fireql.New("<GCP_PROJECT_ID>")
    //OR
    fql, err = fireql.New("<GCP_PROJECT_ID>",
    fireql.OptionServiceAccount("<SERVICE_ACCOUNT_JSON>"))
    if err != nil {
        panic(err)
    }
    
    result, err := fql.
        Execute("SELECT * `users` order by id desc limit 10")
    if err != nil {
        panic(err)
    }
    _ = result
}
```

<!--
### Command-Line
-->

## Some cool `SELECT` queries
```sql
select * from users
select *, id as user_id from users
select id, email as email_address, `address.city` AS city from `users`
select * from users order by 'address.city' desc limit 10
select * from `users`
select id, LENGTH(contacts) as total_contacts from `users`
```
See [Wiki](https://github.com/pgollangi/FireQL/wiki) for more examples and sample queries.

### Authentication

`fireql.NewL` assume Google [Application Default Credentials](https://cloud.google.com/docs/authentication/application-default-credentials) to authenticate to Firestore database if `serverAccount` not passed. Otherwise use service account for authentication.

## Installation

### Go

First, use `go get` to install the latest version of the library.
```bash
go get -u github.com/pgollangi/fireql@latest

```
Next, include `FireQL` in your application:
```go
import "github.com/pgollangi/fireql"
```
<!--
### Homebrew

### Scoop (for windows)

### Manual
You can alternately download suitable binary for your OS at the [releases page](https://github.com/pgollangi/fireql/releases).
-->
## Limitations
All of [firestore query limitations](https://firebase.google.com/docs/firestore/query-data/queries#query_limitations) are applicable when running queries using `FireQL`.

In additional to that:

- Only `SELECT` queries for now. Support for `INSERT`, `UPDATE`, and `DELETE` might come in the future.
- Only `AND` conditions supported in `WHERE` clause. 
- No support for `JOIN`s.
- `LIMIT` doesn't accept an `OFFSET`, only a single number.
- No support of `GROUP BY` and aggregate function `COUNT`.

## Future scope

- [ ] Create interactive command-line shell to run queries
- [ ] Expand support all logical conditions in `WHERE` clause by internally issuing multiple query requests to Firestore and merge results locally before returning.
- [ ] `GROUP BY` support
- [ ] Support other DML queries: `INSERT`, `UPDATE`, and `DELETE`


## Contributing
Thanks for considering contributing to this project!

Please read the [Contributions](https://github.com/pgollangi/.github/blob/main/CONTRIBUTING.md) and [Code of conduct](https://github.com/pgollangi/.github/blob/main/CODE_OF_CONDUCT.md).

Feel free to open an issue or submit a pull request!

## Licence

`FireQL` is open-sourced software licensed under the [MIT](LICENSE) license.
