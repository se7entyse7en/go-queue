# go-queue [![GoDoc](https://godoc.org/gopkg.in/src-d/go-queue.v0?status.svg)](https://godoc.org/github.com/src-d/go-queue) [![Build Status](https://travis-ci.org/src-d/go-queue.svg)](https://travis-ci.org/src-d/go-queue) [![Build status](https://ci.appveyor.com/api/projects/status/15cdr1nk890qpk7g?svg=true)](https://ci.appveyor.com/project/mcuadros/go-queue) [![codecov.io](https://codecov.io/github/src-d/go-queue/coverage.svg)](https://codecov.io/github/src-d/go-queue) [![Go Report Card](https://goreportcard.com/badge/github.com/src-d/go-queue)](https://goreportcard.com/report/github.com/src-d/go-queue)

Queue is a generic interface to abstract the details of implementation of queue
systems.

Similar to the package [`database/sql`](https://golang.org/pkg/database/sql/),
this package implements a common interface to interact with different queue
systems, in a unified way.

Currently, only AMQP queues and an in-memory queue are supported.

Installation
------------

The recommended way to install *go-queue* is:

```
go get -u gopkg.in/src-d/go-queue.v0/...
```

Usage
-----

This example shows how to publish and consume a Job from the in-memory
implementation, very useful for unit tests.

The queue implementations to be supported by the `NewBroker` should be imported
as shows the example.

```go
import (
    ...
	"gopkg.in/src-d/go-queue.v0"
	_ "gopkg.in/src-d/go-queue.v0/memory"
)

...

b, _ := queue.NewBroker("memory://")
q, _ := b.Queue("test-queue")

j, _ := queue.NewJob()
if err := j.Encode("hello world!"); err != nil {
    log.Fatal(err)
}

if err := q.Publish(j); err != nil {
    log.Fatal(err)
}

iter, err := q.Consume(1)
if err != nil {
    log.Fatal(err)
}

consumedJob, _ := iter.Next()

var payload string
_ = consumedJob.Decode(&payload)

fmt.Println(payload)
// Output: hello world!
```

License
-------
Apache License Version 2.0, see [LICENSE](LICENSE)