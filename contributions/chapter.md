# Contributions

This chapter outlines some of the contributions made by several teams to their open source project.

## Kubernetes

## Mattermost

## Docker

### Pull Requests

Here the effort made to get our Pull Requests to `docker/swarmkit` merged is described.

#### Possible data race in test ([docker/swarmkit#2537](https://github.com/docker/swarmkit/pull/2537))

This was found in the project file `manager/state/store/memory_test.go` at lines 2036-2054.

The issue was a loop variable being used in a `func literal` (anonymous function in Go). Due to the nature of Go, this would lead to unexpected results. [Example from Go reference](https://nanxiao.gitbooks.io/golang-101-hacks/content/posts/functional-literals.html):

```
package main

import (
    "fmt"
    "time"
)

func main() {
    for i := 1; i <= 2; i++ {
        go func() {fmt.Println(i)}()
    }
    time.Sleep(time.Second)
}
```

Expected output here would be:

```
1
2
```

However the output would be:

```
3
3
```

["The cause is the func goroutines don't get the opportunity to run until the main goroutine sleeps, and at that time, the variable i has been changed to 3"](https://nanxiao.gitbooks.io/golang-101-hacks/content/posts/functional-literals.html)

Something similar was happening in the SwarmKit test.
So according to the `Contributions.md` in the SwarmKit repository first an issue was opened ([#2536](https://github.com/docker/swarmkit/issues/2536)) describing the problem.
After the issue was opened, a new branch was created in our fork, with the naming convention mentioned in the SwarmKit `Contributions.md` (#issue-*, in our case `delftswa2018:2536-data-race`).

When the issue was fixed, a Pull Request was opened ([#2537](https://github.com/docker/swarmkit/pull/2537)) which was accepted several hours later by [@aaronlehmann](https://github.com/aaronlehmann).

#### Add company and date to license ([docker/swarmkit#2589](https://github.com/docker/swarmkit/pull/2589))

The LICENSE of the project missed the name of the company and the date. Without these fields the LICENSE is not valid and does not apply to the project.

We added these fields and submitted a PR. The PR has been approved and merged, with one small change of squashing to commits into one.

#### Upgrade containerd executor ([docker/swarmkit#2595](https://github.com/docker/swarmkit/pull/2595))

In PR #2568 the "containerd" executor was removed, because it targeted an older version of containerd which was not being maintained anymore.

This PR aims to reintroduce the containerd executor (v1.0.2). This version is being actively maintained.

Currently the PR is under review by the team. There is one comment on the PR stating that instead of using a static version of containerd, just use the master of containerd such that the vendor will always be up to date.

### Issues

### Make script fails on go version go1.10 darwin/amd64 ([docker/swarmkit#2563](https://github.com/docker/swarmkit/pull/2563))

There was an issue with the `certificate-transparency` in go1.10 darwin/amd64. When running the `make` command the following error appeared.

```text
# github.com/docker/swarmkit/vendor/github.com/google/certificate-transparency/go/x509
vendor/github.com/google/certificate-transparency/go/x509/root_darwin.go:73: cannot use nil as type _Ctype_CFDataRef in assignment
make: *** [bin/swarmd] Error 2
```

The fix for the error is updating the `certificate-transparency` vendor to `certificate-transparency-go` and getting the latest version. As the issue with the nil assignment has been fixed in [google/certificate-transparency-go#155](https://github.com/google/certificate-transparency-go/pull/155).

## OSU

## React

## Loopback

## Electron

## Akka

## ElasticSearch

## Spark

## Jenkins

## AngularJS

## TypeScript

## Eden

## Lighthouse

## Phaser

## Xmage

## Mbedos

## Vue.js

## Godot

## Three.js
