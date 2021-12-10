# Chapter 1: Reliable, Scalable, and Maintainable Applications

we focus on three concerns that are important in most software systems
- Reliability
- Scalability
- Maintainability
## Reliability

Correctness encapsulates the following:

- The application performs as expected.
- It can tolerate user mistakes.
- Performance is good enough for a feature under the expected 
- The system prevents any unauthorized access and abuse.

In summary a reliable system continues working even if something goes wrong.

### Faults 
Things that can go wrong. And system that can cope with faults are called fault tolerant systems/ resilient.

A fault is when a specific part of the system has failts, failure is when the entire system crashes. Tolerating faults is preferred
over prevention but in case of securtiy prevention is better.

### Hardware faults
Hard disk crashes, RAM and Grid failures. Machines fail all the time, redundancy is a common way to mitigate failure in hardware.
There's a move to systems with that can function even if some machines completely fail. This also has an operations advantage
since we do not need to shut down the entire system

### Software Errors
- A systamatic error within the system. It can happen due to multiple reasons.
- A software bug that causes every instance of the app server to crash under a bad input.
- A runaway process that used a shared resource.
- A service with a lot of dependants that slows down and essentially causes the entire system to fail.
- Cascading failure, one failure tickle downs.

### Human Errors
Several approaches to avoid human errors:
- Minimize oppurtunities for erros by designing APIS, public interfaces athat encourage the "right thing" and discourage the wrong thing.
 However do not make things to restrictive otherwise people will find a way around it.
- Create sandbox environments for people to test the system out with real data without effecting users.
- Test thouroughly, from unit testing to manual to end to end along with automation testing to find critical egde cases.
- Set up detailed and clear monitoring, such as performance metrics and error rates.
- Implement good management practices and training

## Scalability:
The ability of a system to handle increased load.
Load depends on the system, it can be throughput in batch jobs or response times of APIS. At a lower level it can be database reads 
and writes.

### Example
Twitter measurs performance as:
- Posting tweets(4.6k requests/sec on average, over 12k requests/sec at peak)
- Home timelines(300k requests/sec)

Simply handling 12000 writes is not hard, it's the fan out of tweets to followers that is a challenge, two approaches that tweeter adopted
incrementally:
- First was to simply write a complex join to get the tweets from the tweet table real time. This not scalable

```
SELECT tweets.*, users.* FROM tweets
JOIN users ON tweets.sender_id = users.id
JOIN follows ON follows.followee_id = users.id
WHERE follows.follower_id = current_user
```


- Second approach was to add a tweet to users followers unique feed cache.

The second apporach was more scalable but not suitable for people with a large number of followers.
In the end tweeter adopted a hybrid approach of the two. For people with large followings they avoided adding theri tweets to cache. But
loaded them real time in followers feeds, Rest of the tweets were loaded from cache.


### Measuring performance
We need a way to measure performance. One way is average response time.
A better approach is to look at the median, let's say the median response time is 100 ms, that means half of the responses are faster
then 100 ms.
Median is the 50th percentile. 90th percenitle is similar, let's say the 90th percentile response time is 400 ms, that means that 90% of
requests are faster then 400 ms.

### Handling load
Each large application is different, so the architecture needed to scale it will be different. Some situations need vertical scaling
while sometimes we need horizontal scaling or a combination of both.


## Maintainability
### Operability
Make it easy for operations teams to keep the system running smoothly.
### Simplicity
Make it easy for new engineers to understand the system, by removing as much
complexity as possible from the system. (Note this is not the same as simplicity
of the user interface.)
### Evolvability
Make it easy for engineers to make changes to the system in the future, adapting
it for unanticipated use cases as requirements change. Also known as extensibility,
modifiability, or plasticity.