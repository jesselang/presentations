# Go Modules
### Package Dependency Management

---

###  In The Beginning There Was GOPATH

And it worked.

```text
$GOPATH
  bin/
    helloworld
  src/
    github.com/
      go-training/
        helloworld/
          LICENCE
          README.md
          main.go
          main_test.go
```

---

### GOPATH Enabled Some Great Tooling

```sh
# Get and test the latest version of a Hello World app
# and then run it!
% go get -u github.com/go-training/helloworld
% go test -v github.com/go-training/helloworld
=== RUN   TestHelloWorld
--- PASS: TestHelloWorld (0.00s)
PASS
ok      github.com/go-training/helloworld       0.003s
% go run github.com/go-training/helloworld
Hello World!!

# See the documentation for Cobra
% go doc github.com/spf13/cobra
```

---

### Use a Single GOPATH? As if...

Software is complex, and can be volatile as it moves quickly. The Go committee
encouraged us to write backward-compatible code, but we all know there are
limits to that ideal.

Six months from now, when your company is suffering an outage and you need
to push a critical fix, your app will inevitably fail to build unless...

---

... you can identify and retrieve the

**exact same packages**

that you built your app with when you last deployed it.

(╯°□°）╯︵ ┻━┻

Easy! There are plenty of ways to solve this.

No silver bullets though.

---

### Ways to Solve Dependencies

- GOPATH per app, per app version (please don't do this)
- Vendor your dependencies (not fun to manage, makes for messy PRs)
- Track which git commits of your dependencies were used
- Use tools to automate vendoring or pinning

---

### Super Brief History of Dependency Tooling

- 2015: Go 1.5 introduced `vendor` directory support, `govendor` tool
- Feb 2016: Go 1.6 released with `vendor` support enabled by default
- And then: glide, godep, dep, then vgo.
- Blah, blah, blah, I didn't get into this deep enough to say enough smart things

---

OK Jesse. Enough details already.

How can I effectively use Go modules?

_I'm so glad you asked!_

Let's talk about modules.

---

### Go Modules

A module is a group of Go packages that share a versioning and release
lifecycle.

Modules record precise dependency requirements and create reproducible builds.

Go prefers that modules use [Semantic Versioning][semantic-version].

That is deceptively simple.

[semantic-version]: https://semver.org/

---

### Have Module, Will Release

```sh
% go get -u github.com/spf13/cobra/cobra
% cobra init \
  --pkg-name github.com/jesselang/go-module-example/foocli
% go mod init github.com/jesselang/go-module-example/foocli

# get all dependencies and build
% go build

```

---

### Importing from popular dependency tooling

`go mod init`

Works with vgo, dep, godep, glide, govendor, etc.

---

### Cool, now what?

```sh
# package-building commands add new dependencies
# to go.mod as needed.
% go build
% go test
% go list -m all  # prints the current module’s dependencies.
% go mod tidy # removes unused dependencies.
```
---

### Developing With Multiple Modules

When hacking on a dependent module, you can use local modifications by adding
the `replace` directive to your `go.mod` to point to an on-disk path:

```text
replace example.com/project/foo => ../foo
```

---

### Vendor Directory Support

```sh
# Retrieves all dependencies from go.mod and puts them
# in the vendor/ directory. You can commit those dependencies in your repo
# if you need that guarantee.
% go mod vendor
```

---

### Questions?

@jesselang on Twitter

https://github.com/jesselang/presentations

---

# Links

- [Using Go Modules][go-blog-modules]
- [The Saga of Go Dependency Management][saga]
- [A Gentle Introduction to Golang Modules][gentle-intro]
- [Package Management Tools][package-management]
- [Just tell me how to use Go Modules][just-tell-me]

[go-blog-modules]: https://blog.golang.org/using-go-modules
[saga]: https://blog.gopheracademy.com/advent-2016/saga-go-dependency-management/
[gentle-intro]: https://ukiahsmith.com/blog/a-gentle-introduction-to-golang-modules/
[package-management]: https://github.com/golang/go/wiki/PackageManagementTools
[just-tell-me]: https://www.kablamo.com.au/blog/2018/12/10/just-tell-me-how-to-use-go-modules
