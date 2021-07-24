# Jobs and Cronjobs

Lets talk about jobs and cronjobs. First, I want to cover why these might be useful at a high level, because if you are like me when you first heard of these they where a bit confusing as to why you might want or use them in a kubernetes cluster.

So I think everyone knows what a cronjob is, and in kubernetes that is pretty much the same! Now, just like pods and deployments Cronjobs manage jobs, and jobs manage running pods. So, you could run a pod directly with the `run` command, But you could use a job that would let your run one or more till any number of successful runs and tracks all of this across all the pods running. Meaning you could use a job to spin up 1000 parallel jobs if you want. Jobs also manage the restarting of pods if they exit with a failure. CronJobs then just let you start jobs on a schedule when you want them!

Why is this useful? well now you have the power of kubernetes to do the things that usually one machine would be responsible for. and if that machine was offline, your cronjobs would no longer run. Not only this it gives you the ability to run at massive parallelism with ease. Meaning if you wanted to spin up thousands of parrelel jobs when your cluster was not doing much to process some data that was able to be chunked you could. Think of maintenance tasks like archiving data or cleaning up. What if you wanted to run some type of export or auto save every few minutes, think things like this. Ok. Lets create a cronjob!

`kubectl create cronjob printer --image=busybox --dry-run=client --schedule="0 0 * * *" -o yaml > cronjob.yaml` The creation of this is the exact same as deployment with the exception that you need to use the `--schedule` flag to pass how often to run the job. As you can see here I have said to run it once a day at midnight.

If you open this file up you can see that this job will just run a busybox image. There is not too much to this file and looks a lot like a Deployment, It even shares the characteristics of a deployment where it uses replica-sets internally but you don't need to worry about making a spec for it. If you notice, just like a Deployment you give a spec for a container but then the rest is all about the cronjob. Here we really only specify two options, the restartPolicy and the Schedule

```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  creationTimestamp: null
  name: printer
spec:
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: printer
    spec:
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
          - image: busybox
            name: printer
            resources: {}
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
      parallelism: 3
  schedule: 0 0 * * *
status: {}
```

As we learned in the pods lesson you can override the command the container starts with, lets do that now to have this print! add
```
- /bin/sh
- -c
- date; echo Hello from the Kubernetes cluster
```

now. you could apply this, and feel free too. it will not run until midnight though! so lets make this job run once a minute.

So update the schedule to "* * * * *" this will run it once every minute so that we can see how to get logs, and see what containers are created!

You can get the job created for the cronjob by running
`kubectl get jobs`

And then if you want to find the pods that are run by this job then you can run something like:

`kubectl get pods --selector=job-name=[jobname]`

Aaaannd Thats it. really this is just another simple extension over the pod that lets you manage jobs that run to completion and to do it on a schedule if you so want.
