## ONOS Installation tutorial on Ubuntu 16.04/18.04

### Obtaining the source
Install Java, git, vim, curl

```
$ sudo apt install git git-review openjdk-8-jdk vim curl
```

### Installing bazel
```
$ echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
$ curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
$ sudo apt-get update && sudo apt-get install bazel
$ sudo apt-get install --only-upgrade bazel
```

### Obtain source code and build onos
```
$ git clone https://gerrit.onosproject.org/onos
$ cd onos
$ bazel build onos
$ bazel run onos-local
```

### Run ONOS client on second terminal
```
$ tools/test/bin/onos localhost
Browse http://localhost:8181/onos/ui
```
