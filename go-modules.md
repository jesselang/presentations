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

```text
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

... you can identify and retrieve the packages that you built your app with
when you last deployed it.

Easy! There are plenty of ways to solve this.

---

### Ways to Solve Dependencies

- GOPATH per app, per build (please don't do this)
- Vendor your dependencies (not fun to manage, makes for messy PRs)
- Track which git commits of your dependencies were used
- Use tools to automate vendoring or pinning

---

### Super Brief History of Dependency Tooling

- 2015: Go 1.5 introduced `vendor` directory support, `govendor` tool
- Feb 2016: Go 1.6 released with `vendor` directory support
- And then: glide, godep, dep, then vgo.
- Blah, blah, blah, I didn't get into this deep enough to say enough smart things

---

OK Jesse. Don't bore me with details, how can I effectively use Go modules?

---


# Links

- [The Saga of Go Dependency Management][saga]
- [Package Management Tools][package-management]
- [Just tell me how to use Go Modules][just-tell-me]

[saga]: https://blog.gopheracademy.com/advent-2016/saga-go-dependency-management/
[package-management]: https://github.com/golang/go/wiki/PackageManagementTools
[just-tell-me]: https://www.kablamo.com.au/blog/2018/12/10/just-tell-me-how-to-use-go-modules
