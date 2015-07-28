[![godoc](http://img.shields.io/badge/godoc-reference-blue.svg?style=flat)](https://godoc.org/github.com/efigence/go-raidman)

Raidman
=======

Go Riemann client

```go
package main

import (
        "github.com/efigence/go-raidman"
)

func main() {
        c, err := raidman.Dial("tcp", "localhost:5555")
        if err != nil {
                panic(err)
        }

        var event = &raidman.Event{
                State:   "success",
                Host:    "raidman",
                Service: "raidman-sample",
                Metric:  100,
                Ttl:     10,
        }

        // send one event
        err = c.Send(event)
        if err != nil {
                panic(err)
        }

        // send multiple events at once
        err = c.SendMulti([]*raidman.Event{
                &raidman.Event{
                        State:   "success",
                        Host:    "raidman",
                        Service: "raidman-sample",
                        Metric:  100,
                        Ttl:     10,
                },
                &raidman.Event{
                        State:   "failure",
                        Host:    "raidman",
                        Service: "raidman-sample",
                        Metric:  100,
                        Ttl:     10,
                },
                &raidman.Event{
                        State:   "success",
                        Host:    "raidman",
                        Service: "raidman-sample",
                        Metric:  100,
                        Ttl:     10,
                },
        })
        if err != nil {
                panic(err)
        }

        events, err := c.Query("host = \"raidman\"")
        if err != nil {
                panic(err)
        }

        if len(events) < 1 {
                panic("Submitted event not found")
        }

        c.Close()
}

```
