# Contributing Guide

- [Contributing Guide](#contributing-guide)
  - [Participating in the Project](#participating-in-the-project)
    - [Community Participant](#community-participant)
    - [Contributor](#contributor)
    - [Maintainer](#maintainer)
  - [Ways to Contribute](#ways-to-contribute)
  - [Find an Issue](#find-an-issue)
  - [Ask for Help](#ask-for-help)
  - [Pull Request Lifecycle](#pull-request-lifecycle)
  - [Development Environment Setup](#development-environment-setup)
    - [Linter](#linter)
    - [Build](#build)
    - [Unit Testing](#unit-testing)
    - [E2E Testing](#e2e-testing)
  - [Sign Your Commits](#sign-your-commits)
    - [DCO](#dco)
  - [Pull Request Checklist](#pull-request-checklist)
    - [Build](#build-1)
    - [Lint](#lint)
    - [Testing](#testing)
      - [Unit Testing - Parallel by Default](#unit-testing---parallel-by-default)
      - [E2E Testing](#e2e-testing-1)
    - [Go File Naming](#go-file-naming)

Welcome! We are glad that you want to contribute to our project! 💖

As you get started, you are in the best position to give us feedback on areas of
our project that we need help with including:

* Problems found during setting up a new developer environment
* Gaps in our Quickstart Guide or documentation
* Bugs in our automation scripts

If anything doesn't make sense, or doesn't work when you run it, please open a
bug report and let us know!

Thanks to the maintainers of the [CNCF Project Template Repository](https://github.com/cncf/project-template) for the great work they have done.

## Participating in the Project

There are a number of ways to participate in this project. As the project evolves and grows, we will define a more formal governance model. For now, this document describes various ways community members might participate.

### Community Participant

A Community Participant engages with the project and its community, contributing their time, thoughts, etc. Community participants are usually users who have stopped being anonymous and started being active in project discussions.

### Contributor

A Contributor contributes directly to the project. Contributions need not be code. People at the Contributor level may be new contributors, or they may only contribute occasionally.

### Maintainer

Maintainers are established contributors who are responsible for the entire project. As such, they have the ability to approve PRs against any area of the project, and are expected to participate in making decisions about the strategy and priorities of the project.

## Ways to Contribute

We welcome many different types of contributions including:

* New features
* Builds, CI/CD
* Bug fixes
* Documentation
* Issue Triage
* Communications / Social Media / Blog Posts
* Release management

## Find an Issue

We have good first issues for new contributors and help wanted issues suitable
for any contributor. [good first issue](TODO) has extra information to
help you make your first contribution. [help wanted](TODO) are issues
suitable for someone who isn't a core maintainer and is good to move onto after
your first pull request.

Sometimes there won’t be any issues with these labels. That’s ok! There is
likely still something for you to work on. If you want to contribute but you
don’t know where to start or have an idea, feel free to open a new issue in Github for brainstorming.

Once you see an issue that you'd like to work on, please post a comment saying
that you want to work on it. Something like "I want to work on this" is fine.

## Ask for Help

The best way to reach us with a question when contributing is to ask on the original github issue.

## Pull Request Lifecycle

Generally a comment should be resolved by the one who leaves the comment.

For PR authors, if a comment is not left by you, please do not resolve it even after applying the changes suggested by it. This is to make sure that the changes do address the concern of the PR reviewer as there could be misunderstanding between PR authors and PR reviewers. However, if the PR reviewer is not responding to the comment for whatever reason, the project maintainers can help resolve the comment to unblock the PR author.

For PR reviewers, after a comment left by you is acted upon, it is encouraged to either reply to it or resolve it in a timely manner to unblock the PR author because all the comments are required to be resolved before a PR can be merged. For project maintainers, please target handling unresolved comments within 2 working days.

We feel spelling these norms out is better than assuming them, and we all acknowledge life happens and these are guidelines, not strict rules.


## Development Environment Setup

This section describes how one can develop Finch CLI locally on macOS, build it, and then run it to test out the changes. The design ensures that the local development environment is isolated from the installation (i.e., we should not need to run `make install` to do local development).

### Linter

We use [golangci-lint](https://github.com/golangci/golangci-lint).

To integrate it into your IDE, please check out the [official documentation](https://golangci-lint.run/usage/integrations/).

For more details, see [`.golangci.yaml`](./.golangci.yaml) and the `lint` target in [`Makefile`](./Makefile).

### Build

After cloning the repo, run `make` to build the binary.

The binary in _output can be directly used. E.g. initializing the vm and display the version
```
./_output/bin/finch vm init

./_output/bin/finch version
```

You can run `make install` to make finch binary globally accessible.


### Unit Testing

To run unit test locally, please run `make test-unit`. Please make sure to run the unit tests before pushing the changes.

Ideally each go file should have a test file ending with `_test.go`, and we should have as much test coverage as we can.

To check unit test coverage, run `make coverage` under root finch-cli root directory.


### E2E Testing

Run these steps at the first time of running e2e tests

VM instance is not expected to exist before running e2e tests, please make sure to remove it before going into next step:
```sh
./_output/bin/finch vm stop
./_output/bin/finch vm remove
```

To run e2e test locally, please run `make test-e2e`. Please make sure to run the e2e tests or add new e2e tests before pushing the changes.


## Sign Your Commits

### DCO
Licensing is important to open source projects. It provides some assurances that
the software will continue to be available based under the terms that the
author(s) desired. We require that contributors sign off on commits submitted to
our project's repositories. The [Developer Certificate of Origin
(DCO)](https://probot.github.io/apps/dco/) is a way to certify that you wrote and
have the right to contribute the code you are submitting to the project.

You sign-off by adding the following to your commit messages. Your sign-off must
match the git user and email associated with the commit.

    This is my commit message

    Signed-off-by: Your Name <your.name@example.com>

Git has a `-s` command line option to do this automatically:

    git commit -s -m 'This is my commit message'

If you forgot to do this and have not yet pushed your changes to the remote
repository, you can amend your commit with the sign-off by running

    git commit --amend -s

## Pull Request Checklist

When you submit your pull request, or you push new commits to it, our automated
systems will run some checks on your new code. We require that your pull request
passes these checks, but we also have more criteria than just that before we can
accept and merge it. We recommend that you check the following things locally
before you submit your code:

### Build

```make```

### Lint
```make lint```

### Testing

#### Unit Testing - Parallel by Default

```make test-unit```

For each unit test case (i.e., in both `TestXXX` and the function passed to `t.Run`), `t.Parallel` should be added by default. It should only be skipped under special situations (e.g., `T.Setenv` is used in that test).

Rationale:

- Each unit test case should be independent from each other, so they should be able to be executed in parallel.
- Adding a `t.Parallel` is not much effort as all the underlying details are handled by Go std lib.
- `t.Parallel` helps us ensure that the test cases are truly independent from each other.
- The running time can (theoretically) only go down.

Keeping a good unit test coverage will be part of pull request review. You can run `make coverage` to self-check the coverage.

#### E2E Testing

```make test-e2e```

See `test-e2e` section in [`Makefile`](./Makefile) for more reference.

If the e2e test scenarios you are going to contribute

- are in generic container development workflow
- can be shared by finch-core by replacing test subject from "finch" to "limactl ..."
- E.g.: pull, push, build, run, etc.

implement them in common-tests repo and then import them in [`./e2e/e2e_test.go`](./e2e/e2e_test.go) in finch CLI and finch-core. The detailed flow can be found [here](https://github.com/runfinch/common-tests#sync-between-tests-and-code).

Otherwise, it means that the scenarios are specific to finch CLI (e.g., version, VM lifecycle, etc.), and you should implement them under `./e2e/` (e.g., `./e2e/version.go`) and import them in `./e2e/e2e_test.go`.

### Go File Naming

Keep file names to one word if possible (e.g., avoid stuttering with package name: prefer `thing/factory.go` over `thing/thing_factory.go`). If there have to be more than one words, use underscores as separators. Do not use hyphens or camelCase.

Rationale: It's more readable (i.e., `complicateddistirbutedsystem` vs `complicated_distributed_system`). Furthermore, the practical reason to avoid underscores as separators is that the suffix may later become either an OS or an architecture, but we think that the potential risk is outweighed by the readability gain.

To add more context, there are some [public discussions](https://github.com/golang/go/issues/36060#issue-535213527) on this, but there is no consensus yet.
