# Jobs and Cronjobs

Lets talk about jobs and cronjobs. I want to first cover why these might be useful at a high level, because if you are like me when you first heard of these they where a bit confusing as to why you might want or use them in a kubernetes cluster.

So I think everyone knows what a cronjob is, and in kubernetes that is pretty much the same! Now, just like pods and deployments Cronjobs manage jobs, and jobs manage running pods. So, you could run a pod directly with the `run` command, you could use a job that would let your run one or more till and number of sucesfful runs and tracks all of this accross all the pods meaning you could use a job to spin up 1000 parrellel jobs if you want. Jobs also manage the restarting of pods if they exit with a failure. CronJobs then just let you start jobs on a scedule and work start jobs when you want them!

Why is this useful? well now you have the power of kubernetes to do the things that usually one machine would be responsible. and if that machine was offline, your cronjobs would no longer run. Not only this it gives you the ability to run at massive parralelism with easy. Meaning if you wanted to spin up thousands of parrelel jobs when your cluster was not doing much to process some data that was able to be chuncked. Ok. Lets create a cronjob!

`kubectl create cronjob printer --image=busybox --dry-run=client --schedule="0 0 * * *" -o yaml > cronjob.yaml` The creation of this is the exact same as deployment with the exception that you need to use the `--schedule` flag to pass how offten to run the job. As you can see here I have said to run it once a day at midnight.

If you open this file up you can see that this job with just run a busybox image. Now we want it to acctually do something so lets make this print something out!

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
  schedule: 0 0 * * *
status: {}
```

As we learned in the pods lesson you can override the command the container starts with, lets do that now to have this print! add
```
- /bin/sh
- -c
- date; echo Hello from the Kubernetes cluster
```

now. you could apply this, and feel free too. it will not run until might though! Feel free to change it too sooner if you want too.

To then see all the pods that are used for this cronjob you can run
You can get the job created for the cronjob by running
`kubectl get jobs`

if you want to find the pods that are run by this job then run

`kubectl get pods --selector=job-name=[jobname]`

Aaaannd Thats it. really this is just another simple extention over the pod that lets you manage jobs that run to completion!
