# Logging Workload

Stress test the cluster logging and ElasticSearch operators.

## Requirements

* Cluster logging operator is deployed
* kubeconfig present in $HOME/.kube/config on orchestration host
* podman and libselinux-python2 installed on orchestration host
* selinux set to permissive (otherwise cluster-loader will fail)
* clusterlogging instance is "Unmanaged"
* fluent.conf is modifed to exclude all infra logs (e.g. journal and containers from openshift*, kube*, default)

## Run from CLI

```shell script
cp workloads/inventory.example inventory
# Add orchestration host to inventory
# Define environmental variables or use default logtest parameters
ansible-playbook -v -i inventory workloads/logging.yml
```

## Implementation Notes
Currently I am templating parameters directly into logtest-rc.json because 
I ran into issues trying to get cluster-loader to pass in the values from logtest.yml. 
I left in the step where the variables are templated into logtest.yml in case anything changes.

## Ansible Variables

### LABAL_ALL_NODES
default: False  
If False: Remove all placement=logtest labels from worker nodes then add it back to just 1 worker node.  
If True: Apply placement=logtest label to all worker nodes

## Environmental Variables

### ORCHESTRATION_USER

default: `root`  
Remote user to connect as.

### LABAL_ALL_NODES

default: `False`  
If True, label all nodes with placement=logtest.  
If False, remove placment label from all worker nodes and only add it back to 1.

### PROJECT_BASENAME

default: `logtest-`  
Basename for project creation

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
