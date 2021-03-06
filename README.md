# libcompose

[![GoDoc](https://godoc.org/github.com/docker/libcompose?status.png)](https://godoc.org/github.com/docker/libcompose)
[![Jenkins Build Status](https://jenkins.dockerproject.org/view/Libcompose/job/Libcompose%20Master/badge/icon)](https://jenkins.dockerproject.org/view/Libcompose/job/Libcompose%20Master/)

A Go library for Docker Compose. It does everything the command-line tool does, but from within Go -- read Compose files, start them, scale them, etc.

**Note: This is experimental and not intended to replace the [Docker Compose](https://github.com/docker/compose) command-line tool. If you're looking to use Compose, head over to the [Compose installation instructions](http://docs.docker.com/compose/install/) to get started with it.**

```go
package main

import (
	"log"

	"github.com/docker/libcompose/docker"
	"github.com/docker/libcompose/project"
)

func main() {
	project, err := docker.NewProject(&docker.Context{
		Context: project.Context{
			ComposeFiles: []string{"docker-compose.yml"},
			ProjectName:  "my-compose",
		},
	})

	if err != nil {
		log.Fatal(err)
	}

	project.Up()
}
```

## Building

You need either [Docker](http://github.com/docker/docker) and `make`,
or `go` in order to build libcompose.

### Building with `docker`

You need Docker and ``make`` and then run the ``binary`` target. This
will create binary for all platform in the `bundles` folder. 

```bash
$ make binary
docker build -t "libcompose-dev:refactor-makefile" .
# […]
---> Making bundle: binary (in .)
Number of parallel builds: 4

-->      darwin/386: github.com/docker/libcompose/cli/main
-->    darwin/amd64: github.com/docker/libcompose/cli/main
-->       linux/386: github.com/docker/libcompose/cli/main
-->     linux/amd64: github.com/docker/libcompose/cli/main
-->       linux/arm: github.com/docker/libcompose/cli/main
-->     windows/386: github.com/docker/libcompose/cli/main
-->   windows/amd64: github.com/docker/libcompose/cli/main

$ ls bundles
libcompose-cli_darwin-386*    libcompose-cli_linux-amd64*      libcompose-cli_windows-amd64.exe*
libcompose-cli_darwin-amd64*  libcompose-cli_linux-arm*
libcompose-cli_linux-386*     libcompose-cli_windows-386.exe*
```


### Building with `go`

- You need `go` v1.5
- You need to set export `GO15VENDOREXPERIMENT=1` environment variable
- If your working copy is not in your `GOPATH`, you need to set it
accordingly.

```bash
$ go generate
# Generate some stuff
$ go build -o libcompose ./cli/main
```


## Running

A partial implementation of the libcompose-cli CLI is also implemented in Go. The primary purpose of this code is so one can easily test the behavior of libcompose.

Run one of these:

```
libcompose-cli_darwin-386
libcompose-cli_linux-amd64
libcompose-cli_windows-amd64.exe
libcompose-cli_darwin-amd64
libcompose-cli_linux-arm
libcompose-cli_linux-386
libcompose-cli_windows-386.exe
```

## Tests (unit & integration)


You can run unit tests using the `test-unit` target and the
integration test using the `test-integration` target. If you don't use
Docker and `make` to build `libcompose`, you can use `go test` and the
following scripts : `script/test-unit` and `script/test-integration`.

```bash
$ make test-unit
docker build -t "libcompose-dev:refactor-makefile" .
#[…]
---> Making bundle: test-unit (in .)
+ go test -cover -coverprofile=cover.out ./docker
ok      github.com/docker/libcompose/docker     0.019s  coverage: 4.6% of statements
+ go test -cover -coverprofile=cover.out ./project
ok      github.com/docker/libcompose/project    0.010s  coverage: 8.4% of statements
+ go test -cover -coverprofile=cover.out ./version
ok      github.com/docker/libcompose/version    0.002s  coverage: 0.0% of statements

Test success
```


## Current status

The project is still being kickstarted... But it does a lot.  Please try it out and help us find bugs.

## Contributing

Want to hack on libcompose? [Docker's contributions guidelines](https://github.com/docker/docker/blob/master/CONTRIBUTING.md) apply.
