# Logging Workload

Stress test the cluster logging and ElasticSearch operators.

## Requirements

* Cluster logging operator is deployed
* kubeconfig present in $HOME/.kube/config on orchestration host
* podman and libselinux-python2 installed on orchestration host
* selinux set to permissive (otherwise cluster-loader will fail)

## Run from CLI

```shell script
cp workloads/inventory.example inventory
# Add orchestration host to inventory
# Define environmental variables or use default logtest parameters
ansible-playbook -v -i inventory workloads/logging.yml
```

## Environmental Variables

### ORCHESTRATION_USER

default: `root`  
Remote user to connect as.

### NUM_PROJECTS

default: `1`  
Number of logtest projects to create.

### NUM_LINES

default: `1800000`  
Number of lines for logtest to generate per project.

### LINE_LENGTH

default: `1024`  
Length of log lines in bytes.

### RATE

default: `60000`  
Messages per second for logtest pod to generate.

### PAUSE_OFFSET

default: `5`  
Time to wait (in minutes) after logtest is done before verifying all messages showed up in elasticsearch.

### ORIGIN_TESTS_VERSION

default: `latest`  
Version of quay.io/openshift/origin-tests to pull.