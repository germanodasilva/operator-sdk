---
title: Testing your Operator project
linkTitle: Testing with EnvTest
description: Learn how to ensure the quality of your Operator project 
weight: 70
---

## Overview

The Operator SDK project recommends using controller-runtime's [envtest][envtest] to write tests for your Operators projects. Envtest has a more active contributor community, it is more mature than Operator SDK's test framework, and it does not require an actual cluster to run tests which can be a huge benefit in CI scenarios.

## Using EnvTest

You will see that `controllers/suite_test.go` is created when a controller is scaffolded by the tool. This file contains boilerplate for executing integration tests using [envtest][envtest] with [ginkgo](https://onsi.github.io/ginkgo/) and [gomega][gomega].
Setup instructions, including those for disconnected environments, are found [here][envtest-setup].

These tests are runnable as native Go tests:

```shell
go test controllers/ -v -ginkgo.v
```

The projects generated by using the SDK tool have a Makefile which contains the target tests which executes when you run `make test`. Note that this target will also execute when you run `make docker-build IMG=<some-registry>/<project-name>:<tag>`. 

Operator SDK adopted this stack to write tests for its operators. It might be useful to check [writing controller tests][writing-controller-tests] documentation and examples to learn how to better write tests for your operator. See, for example, that [controller-runtime](https://github.com/kubernetes-sigs/controller-runtime) is covered by tests using the same stack as well. 

## e2e Integration tests

- **For Golang-based operators**: you can create the e2e tests using Go. See the `test` directory for the Memcached sample 
under the [testdata/go/v3/memcached-operator][sample] to see an example of e2e tests.
- **For Ansible-based operators**: you can use [Molecule][molecule], an Ansible testing framework. For further information see [Testing with Molecule][molecule-tests].
- **For Helm-based operators**: you can also use [Chart tests][helm-chart-tests].

Alternatively, you can achieve the same goal using shell scripts. The following are a few examples of shell scripts
used for testing projects built with SDK `1.0.0`:

- [Legacy test to check Golang-based Operators][go-legacy-shell]
- [Legacy test to check Helm-based Operators][helm-legacy-shell]
- [Legacy test to check Ansible-based Operators][ansible-legacy-shell]

## Other Options

Also, you can write tests for your operator in a declarative format using [kuttl][kuttl]. Via kuttl, you can define YAML manifests that specify the expected before and after states of a cluster when your operator is used. For more info see [Writing Kuttl Scorecard Tests][writing-kuttl-scorecard-tests].

To implement application-specific tests, the SDK's test harness, [scorecard][scorecard], provides the ability to ship custom code in container images as well, which can be referenced in the test suite. Because this test suite definition metadata travels with the Operator Bundle, it allows for functional testing of the Operator without the source code or the project layout being available. See [Writing Custom Scorecard Tests][writing-custom-scorecard-tests].

[envtest]: https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/envtest
[envtest-setup]:https://book.kubebuilder.io/reference/envtest.html
[writing-controller-tests]: https://book.kubebuilder.io/cronjob-tutorial/writing-tests.html
[writing-kuttl-scorecard-tests]: /docs/testing-operators/scorecard/kuttl-tests
[writing-custom-scorecard-tests]: /docs/testing-operators/scorecard/custom-tests
[scorecard]: /docs/testing-operators/scorecard/
[gomega]: https://onsi.github.io/gomega/
[kuttl]: https://kuttl.dev/
[sample]: https://github.com/operator-framework/operator-sdk/tree/master/testdata/go/v3/memcached-operator
[molecule]: https://molecule.readthedocs.io/en/latest/
[molecule-tests]: /docs/building-operators/ansible/testing-guide
[helm-chart-tests]: https://helm.sh/docs/topics/chart_tests/
[go-legacy-shell]: https://github.com/operator-framework/operator-sdk/blob/v1.0.0/hack/tests/e2e-go.sh
[helm-legacy-shell]: https://github.com/operator-framework/operator-sdk/blob/v1.0.0/hack/tests/e2e-helm.sh
[ansible-legacy-shell]: https://github.com/operator-framework/operator-sdk/blob/v1.0.0/hack/tests/e2e-ansible.sh
