#  Job Types in Kubernetes

In Kubernetes, a job is a controller object that represents a task or a batch job that runs a containerized workload to completion. Jobs create one or more pods and ensure that a specified number of them successfully terminate. They are primarily used for short-lived, non-replicated tasks, such as batch processing, data migration, or running scripts.

There are a few variations of Job types that you can use:


## Types
 ### 1. Single Job
This type of job runs a single task to completion.
Once the task finishes successfully, the job is considered complete. This is also considered a non-parallel job. This Is because, job runs a single task to completion without attempting to run multiple instances concurrently.

**Apply manifest**

>kubectl apply -f simple-job.yaml

**Verify**

> kubectl get job

### 2. Parallel job
    
It is also known as a parallel job or batch job, this type runs multiple identical tasks simultaneously, usually to process a batch of work items in parallel.

**Apply manifest**

>kubectl apply -f parralel-job.yaml

**Verify**

> kubectl get job

### 3. Cron Job
    
 A cron job runs a job on a periodic schedule, It is similar to the cron jobs facility in Unix/Linux systems.
 
**Apply manifest**

>kubectl apply -f cron-job.yaml

**Verify**

> kubectl get cronjob

## Conclusion
In Kubernetes, jobs are used to run short-lived tasks or batch processes. They ensure that a specified number of Pods successfully complete their tasks before terminating. Jobs are ideal for tasks like data processing, batch jobs, or one-time operations that don't require continuous monitoring. They provide mechanisms for task completion, retries, and parallelism, making them suitable for various workload types within Kubernetes deployments.


