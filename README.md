## Tekton Katacoda Tutorial
[https://katacoda.com/tektoncd/](https://katacoda.com/tektoncd/)

<br/>

---
## Step, Task, Pipeline, TaskRun, PipelineRun
![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FciCmoS%2FbtqAqhTWXl6%2FALXIBrHtiCwldyJvRIOjB0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FciCmoS%2FbtqAqhTWXl6%2FALXIBrHtiCwldyJvRIOjB0%2Fimg.png)

---
## Task
```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello
spec:
  steps:
  - name: hello
    image: ubuntu
    script: |
      set -e
      echo "Hello World!"
```
---
## TaskRun
**- Way1. Using kubectl with yaml**
```yaml
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  generateName: hello-run-
spec:
  taskRef:
    name: hello
```

**- Way2. Using tkn (Tekton Client)**
```bash
$ tkn task start hello --dry-run > taskRun-hello.yaml
$ kubectl create -f taskRun-hello.yaml

$ tkn task start hello
```
---
## PipeLine
```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: hello-goodbye
spec:
  tasks:
  - name: hello
    taskRef:
      name: hello
  - name: goodbye
    runAfter: 
      - hello
    taskRef:
      name: goodbye
```
---
## PipeLineRun
**- Way1. Using kubectl with yaml**
```yaml
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  generateName: hello-goodbye-run-
  namespace: default
spec:
  pipelineRef:
    name: hello-goodbye
```

**- Way2. Using tkn (Tekton Client)**
```bash
$ tkn pipeline start hello-goodbye --dry-run > pipelineRun-hello-goodbye.yaml
$ kubectl create -f  pipelineRun-hello-goodbye.yaml

$ tkn pipeline start hello-goodbye
```
