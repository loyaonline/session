# session
A efficient, safely and easy-to-use session library for Go.

## Quick Start

### Download and install

```bash
go get -v github.com/loyaonline/session
```

### Create file `server.go`

```go
package main

import (
	"context"
	"fmt"
	"net/http"

	session "github.com/loyaonline/session"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		store, err := session.Start(context.Background(), w, r)
		if err != nil {
			fmt.Fprint(w, err)
			return
		}

		store.Set("foo", "bar")
		err = store.Save()
		if err != nil {
			fmt.Fprint(w, err)
			return
		}

		http.Redirect(w, r, "/foo", 302)
	})

	http.HandleFunc("/foo", func(w http.ResponseWriter, r *http.Request) {
		store, err := session.Start(context.Background(), w, r)
		if err != nil {
			fmt.Fprint(w, err)
			return
		}

		foo, ok := store.Get("foo")
		if ok {
			fmt.Fprintf(w, "foo:%s", foo)
			return
		}
		fmt.Fprint(w, "does not exist")
	})

	http.ListenAndServe(":8080", nil)
}
```

### Build and run

```bash
go build server.go
./server
```

### Open in your web browser

<http://localhost:8080>

```text
    foo:bar
```

## Features

- Easy to use
- Multi-storage support
- Multi-middleware support
- More secure, signature-based tamper-proof
- Context support
- Support request header and query parameters

## Store Implementations

- [https://github.com/loyaonline/redis-session-store](https://github.com/loyaonline/redis-session-store) - Redis

