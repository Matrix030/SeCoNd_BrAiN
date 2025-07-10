We're building a fully-fledged web server from scratch _on your local machine_. The test suite will make HTTP requests to your local server over [localhost](https://www.hostinger.com/tutorials/what-is-localhost). Your server will run in one terminal, while you submit tests with the [Boot.dev CLI](https://github.com/bootdotdev/bootdev) in another terminal.

## Setup

### Tools You'll Need

1. A code editor. I use [VS code](https://code.visualstudio.com/), but you can use whatever you're comfortable with.
2. A command line. I work on macOS/Linux, so my instructions will be in Bash. I recommend [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/install) if you're on Windows so you can still use Linux commands.
3. The [Go toolchain](https://golang.org/doc/install) with version `1.22+`.
4. The [Boot.dev CLI](https://github.com/bootdotdev/bootdev) to run the tests. Go ahead and install it following the instructions in the README, then run `bootdev login` to authenticate.

The lessons in this course _require_ at least version `1.22` of Go. If you're using an older version, you'll run into some frustrating issues!

### Set up Your Project

Create a new GitHub/GitLab repository for your Chirpy project, and clone it down onto your local machine. Use `go mod init` to create a new Go module for the project, and add a `main.go` file. That's where you'll be writing your code for each assignment.

_Do not delete your work after each assignment_! Each lesson will build upon the previous ones so we'll be reusing a lot of code.

## Assignment

The Go standard library makes it easy to build a simple server. Your task is to build and run a server that binds to `localhost:8080` and always responds with a `404 Not Found` response.

### Steps

1. [ ] Create a [new http.ServeMux](https://pkg.go.dev/net/http#NewServeMux)
2. [ ] Create a new [http.Server](https://pkg.go.dev/net/http#Server) struct.
    - Use the new "ServeMux" as the server's handler
    - Set the `.Addr` field to ":8080"
3. [ ] Use the server's [ListenAndServe](https://pkg.go.dev/net/http#Server.ListenAndServe) method to start the server
4. [ ] Build and run your server (e.g. `go build -o out && ./out`)
5. [ ] Open `http://localhost:8080` in your browser. You should see a `404` error because we haven't connected any handler logic yet. Don't worry, that's what is expected for the tests to pass for now.

**Run and submit** the CLI tests using the [Boot.dev CLI tool](https://github.com/bootdotdev/bootdev) in another terminal window **while your server is still running**.

## Tips

- Use `go mod init` to create a Go module for your project
- Each time you change your code you'll need to rebuild and restart your server
- Use Git to save your work as you go