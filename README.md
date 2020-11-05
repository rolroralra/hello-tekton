## Tekton Katacoda Tutorial
[https://katacoda.com/tektoncd/](https://katacoda.com/tektoncd/)

<br/>

---
## How to Install Tekton
[https://github.com/tektoncd/pipeline](https://github.com/tektoncd/pipeline)
<br/>

```bash
$ kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

$ kubectl get pods -n tekton-pipelines
NAME                                           READY   STATUS              RESTARTS   AGE
tekton-pipelines-controller-6c6ffb8d9c-mpfl4   0/1     ContainerCreating   0          7s
tekton-pipelines-webhook-7594fd4c9b-7w9dz      0/1     ContainerCreating   0          7s
```

---
## How to Install Tekton CLI (tkn)
[https://github.com/tektoncd/cli](https://github.com/tektoncd/cli)

---
## Step, Task, Pipeline, TaskRun, PipelineRun
- [Task](https://github.com/rolroralra/hello-tekton#task)
- [TaskRun](https://github.com/rolroralra/hello-tekton#taskrun)
- [Pipeline](https://github.com/rolroralra/hello-tekton#pipeline)
- [PipelineRun](https://github.com/rolroralra/hello-tekton#pipelinerun)

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

---
## PipelineResource
[https://github.com/tektoncd/website/tree/master/tutorials/katacoda/getting-started/src/tekton-katacoda](https://github.com/tektoncd/website/tree/master/tutorials/katacoda/getting-started/src/tekton-katacoda)
