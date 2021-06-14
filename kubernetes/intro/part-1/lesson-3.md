# What is immutable infrastructure and declarative resources

All of us has heard of "it worked on my machine" but then failed in production because of an un-accounted for veriable. This problem is "solved" by immutable infrastructure. This is mostly thanks to containers but is also enabled by kubernetes as a whole.

While containers provide the base layer it became important to ensure this immutable infrastructure could be run and sceduled at will empowering you to worry less about if your application. Now this solves a lot of issues and hopefully looks to solve.

Now this is great. kubernetes and containers not only provides you the peace of mind that not only is your application or service going to be able to run but it will be able to rescedule it in case of a fault because your container can run on any of the machines in the kubernetes cluster. This also makes things like scalling simpler because you don't need to worry about provisioning a machine if another replica of your applications needs run.

This being said kubernetes addresses a lot of real world problems especially if your application is a microservice. But it causes its own issues, First you must manage kubernetes. Now there are many managed solutions but you still need to manage how your kubernetes cluster is provissioned and you need to understand how to deploy an application on kubernetes. This brings a lot of specialized knowledge and work.

Another issue you need to make sure everything you deploy can be described declarativly, now while I think this is the way to go you have to be understanding of this and ensure all the teams are on board.

