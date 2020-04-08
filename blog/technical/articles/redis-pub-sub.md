[Home](../../../README.md) > Blog > [Technical](../blog-technical.md) <!-- Breadcrumb -->

# Redis PubSub

## The problem with scaling and the cloud

This project was meant to be deployed to the cloud, but what has this to do with this topic?<br>
The cloud allows us to start and stop instances of our micro services in just a few seconds.

This clearly is a great advantage of the cloud, but what if we want to communicate between different instances? In reality it's hard to know the IP of every currently running micro service to try to communicate with them.<br>
Micro services in a cloud start and stop (sometimes crash) without any interaction from our side.


We also have the problem that not every instance wants to know everything currently happening in our infrastructure. Sending notifications to these micro services would mean alot of overhead which increases dramastically every time our infrastructure scales itself up.

Because of that we need a _"central"_ instance knowing:

* The IPs of every micro service and
* What every instance really wants to know

So, we need another micro service, right? No.<br>
A single micro service handling all the things we want to share with other micro services, would mean we have to develop and maintain it.<br>
Even if we would maintain it: What happens if the service itself crashes or gets slow because of millions of messages per minute? This would mean that we wouldn't be able to communicate between our micro services which could lead to 1. data loss and 2. unavailable features or even 3. broken security mechanisms.

## The solution to nearly all our problems

Most tech people know Redis as a cache, but it also provides Pub/Sub features!

Every micro service can be a subscriber and/or publisher.<br>
**Subscribers** want get notifications when something specific is happening. The things they want to know are defined by _topics_.<br>
**Publishers** publish messages with specific _topics_.

The Redis server now gets a message with a specific _topic_ from a **publisher** and distributes it to every **subscriber** in our network that has subscribed to the _topic_ before.

That's the general concept of the Redis PubSub service. Now let's face new problems.

### Central instances and cloud scaling

A single Redis PubSub instance is fast, extremely fast, but even Redis has limitations in terms of the amount of messages per second it can handle.

When desinging cloud infrastructure it's very important to (almost) never limitate your system: Even if you think you won't need that much performance the next few year, what is your plan after these few years? Your infrastructure is built and deployed which means changing something is a huge task and risk for your business.

I wish I could write something like: Redis supports clustering - solution to all our problems!<br>
But the (current) truth is: Redis Clusters are not made for pub/sub which means we would get **linear decreasing** performance when increasing the cluster size.

So, why chose Redis nevertheless? There are architectural workarounds for Redis which allow us to scale our infrastructure without hitting limitations: [This talk is about how one company acheived this](https://youtu.be/6G22a5Iooqk)<br>
Of course I'm aware that even this architecture has some bottlenecks on a big scale, but there are also solutions for these. (e.g. read replications for the discovery service to load-balance read-operations etc.)


[Home](../../../README.md)
