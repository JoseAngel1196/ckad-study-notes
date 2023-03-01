# Jobs and CronJobs

Created: March 1, 2023 1:19 PM

# Introduction to Jobs and CronJobs

## What is a Job?

Let’s consider an application that has been dockerized and essentially, this application is a script that runs once, without the need to remain running continuously. This is where Kubernetes Job comes in handy. It is ideal when we have an application that needs to perform a task and then shut down immediately after completion.

## What is a CronJob?

A Kubernetes CronJob is similar to a Kubernetes Job in concept, but with one key difference: it allows us to run a Job periodically on a set schedule. For example, we could schedule a task to run every day at 3:00 p.m. This makes it a powerful tool for automating repetitive tasks that need to be run at specific intervals.

## What does it look like to create a Job?

| Name | Explanation |
| --- | --- |
| backoffLimit | If the Kubernetes Job fails, Kubernetes will automatically attempt to restart the container. The backoffLimit specifies the maximum number of times Kubernetes should retry running the job. |
| activeDeadlineSeconds | If the container takes too long to execute, Kubernetes will terminate the job, resulting in a failed job status. |
| ttlSecondsAfterFinished | This means that after the Job's Pod completes or fails, the Pod will remain in that state for 1 (the time specified in the YAML) hour before being automatically deleted by the Kubernetes API server. |

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: print
spec:
	ttlSecondsAfterFinished: 3600 # 1 hour.
  template:
    spec:
      containers:
      - name: print
        image: busybox:stable
        command: ["echo", "This is a test!"]
      restartPolicy: Never
  backoffLimit: 4
  activeDeadlineSeconds: 10
```

## What does it look like to create a CronJob?

| Name | Explanation |
| --- | --- |
| schedule | This refers to the specific time at which we want our Kubernetes Job to execute based on a defined schedule. |

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

## Commands

```bash
# Create a Job.
➜ kubectl create job print-job --image=busybox:stable --dry-run=client -o yaml > print-job.yaml

# Create a CronJob

➜ kubectl create cronjob print-cronjob --image=busybox:stable --schedule="*/1 * * * *"  --dry-run=client -o yaml > print-cronjob.yaml
```