# Requirements

1. [Backend](#backend)
   1. [Environment](#environment)
   2. [Getting imported packages](#getting-imported-packages)
   3. [Steps to get protobuf compilation working](#steps-to-get-protobuf-compilation-working)
   4. [Building](#building)
   5. [Upgrading to newer versions of development environments](#upgrading-to-newer-versions-of-development-environments)
2. [CLI](#cli)
3. [Frontend](#frontend)

## Backend

The examples below are for Windows, but corresponding steps also apply for other operating systems.

| Language | Version | Package |
| -------- | ------- | ------- |

It is highly recommended to use [VS Code](https://code.visualstudio.com/).

### Environment

If you are on Windows, I highly recommend installing [msys2](http://www.msys2.org/). After installation, run the following from the msys2 shell:

```plaintext
pacman -Syu
pacman -S base-devel
pacman -S mingw-w64-x86_64-toolchain
pacman -S mingw-w64-x86_64-cmake
```

1. Install Go.
2. Make sure PATH contains `C:\Go\bin`, assuming `C:\Go` is where Go was installed.
3. Add a new environment variable called `GOROOT` with value `C:\Go`.
4. Be sure to set `GOPATH` (user) environment variable variable (`G:\dev\go` as an example on Windows). This is where your Go projects and compiled binaries will reside. Now you have a few options.
   1. Under `G:\dev\go`, Go will create a `src` (Go projects; imported and local) and a `bin` (compiled binaries) directory.
   2. You can put your projects under the `src` directory. The advantage of this approach is in using code internal to the same project. Go does not work very well with relative paths. This approach will help and is recommended. [This article](https://golang.org/doc/code.html#Organization) is useful reading.
   3. Alternately, a different folder could be used, either under `src` or somewhere outside of `GOPATH` (i.e., `G:\dev\go`). In such a case, importing code from another file within the same project would require either using the [vendor](https://stackoverflow.com/a/45813698/420827) folder mechanism, or some other "non-standard" approach.
5. Add `G:\dev\go\bin` (using above example) to PATH.

### Getting imported packages

For all non-native Go packages you have to do a `go get -u`. Check your source files for those imported packages. For example, in api/handler.go, we have imported `golang.org/x/net/context`, so we have to run `go get -u golang.org/x/net/context`.

### Steps to get protobuf compilation working

1. Get protocol buffers from [here](https://github.com/google/protobuf/releases).
2. Add location of local `bin` folder from under protobuf to PATH.
3. Run following (for `google/api/annotations.proto`, imported in api/api.proto)

    ```plaintext
    go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway
    go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger
    go get -u github.com/golang/protobuf/protoc-gen-go
    ```

    For other imports you will have to run `go get -u` similarly.

4. Either of the following commands work, depending on where this project resides

    ```plaintext
    C:\data\devgithub\go\src\github.com\manastalukdar\yet-another-pomodoro-timer>protoc -I %GOPATH%/src/github.com/manastalukdar/yet-another-pomodoro-timer/app/backend/api/ api.proto -I %GOPATH%/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis --go_out=plugins=grpc:./app/backend/api
    ```

    ```plaintext
    C:\data\devgithub\_my-projects\yet-another-pomodoro-timer>protoc -I ./app/backend/api/ api.proto -I %GOPATH%/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis --go_out=plugins=grpc:./app/backend/api
    ```

### Building

```plaintext
From root of: G:\dev\go\src\github.com\manastalukdar\yet-another-pomodoro-timer

go build -i -v -o bin/app/backend/server.exe .\app\backend\server\
go build -i -v -o bin/app/backend/client.exe .\app\backend\client\
```

### Upgrading to newer versions of development environments

## CLI

## Frontend
