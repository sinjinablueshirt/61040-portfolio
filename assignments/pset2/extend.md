# Extending the Design
[back to table of contents](/assignments/pset2/contents.md)

## Concept 1
**concept** ResourceCounter[Resource]

**purpose** to keep a counter with a resource

**principle** once a resource is initialized, a counter can be incremented and observed that is associated with that resource

**state** <br>
&nbsp;&nbsp;a set of Resource with <br>
&nbsp;&nbsp;&nbsp;&nbsp;a count Int

**actions** <br>
&nbsp;&nbsp;initialize(resource: Resource) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** resource exists and is not already recorded<br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** initializes resource with count=0 <br>

&nbsp;&nbsp;increment(resource: Resource) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** resource exists and is recorded in the state <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** increments resource's counter by one

&nbsp;&nbsp;examine(resource: Resource): (count: Int) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** resource exists and is recorded in the state <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** returns the count associated with the resource <br> <br>

## Concept 2
**concept** ResourceOwner[User, Resource]

**purpose** to keep track of which user owns each resource

**principle** Users can be bound to a resource as its owner. It can be later checked that some user is the owner of some resource

**state** <br>
&nbsp;&nbsp;a set of Resource with <br>
&nbsp;&nbsp;&nbsp;&nbsp;an owner User

**actions** <br>
&nbsp;&nbsp;claim(resource: Resource, user: User)<br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** the resource is not already owned by any user <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** makes user own resource

&nbsp;&nbsp;check(resource: Resource, user: User): (resource: Resource) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** the resource is owned by user

***note that check only succeeds if the passed in user owns the resource**


## Sync 1
**sync** setOwner <br>
**when** <br>
&nbsp;&nbsp;Request.shortenUrl(user) <br>
&nbsp;&nbsp;UrlShortening.register (): (shortUrl) <br>
**then** <br>
ResourceOwner.claim(resource: shortUrl, user) <br>
ResourceCounter.initialize(resource: shortUrl)

## Sync 2 (see [notes](#notes))
**sync** count <br>
**when** UrlShortening.lookup (shortUrl): () <br>
**then** ResourceCounter.increment(resource: shortUrl)


## Sync 3.1 and Sync 3.2 (see [notes](#notes))
**sync** check <br>
**when** Request.examineAnalytics(user, shortUrl) <br>
**then** ResourceOwner.check(resource: shortUrl, user)

**sync** analysis <br>
**when** ResourceOwner.check(resource: shortUrl, user) <br>
**then** ResourceCounter.examine(resource: shortUrl)

## Feature Request Analysis

### Allowing users to choose their own short URLs
This one could definitely be added. In the register sync, we would just have to change it so that if the request has a desired suffix by the user then to pass that into the UrlShortening.register action instead of the nonce generated from NonceGeneration.generate. None of the syncs I defined (setOwner, count, check, analysis) need to be changed

### Using the “word as nonce” strategy to generate more memorable short URLs
If it were up to me, I would not include this feature. As I mentioned in a previous part, this behavior could be added by changing the NonceGeneration concept to have an initial "unused" set of words from which to randomly pick from and none of the syncs I defined would . However, as mentioned, this makes the URL shortening less secure and may also be less optimal since we'd need to store every possible word upon initializing the state.

### Including the target URL in analytics, so that lookups of different short URLs can be grouped together when they refer to the same target URL
This feature has one major issue that is how we handle who can access URL analytics. Currently, it is designed such that the user who requested the shortened URL is the only one that can see that URL's statistics. However, if we were to "group" together the analytics of all the shortUrls of a targetUrl, then multiple users will be able to see information about other user's shortUrls when examining their own shortUrl's information, which is not what we intended in our specifications.

### Generate short URLs that are not easily guessed
I'm not really sure what this means by "not easily guessed". I assume that it means using random strings for the short URLs like "jbIAOJSbfiojjas". In this case, the only thing we'd really have to do is specify that the implementation of NonceGeneration generates random unique strings. If we wanted to, we could additionally change the concept spec to be more narrow by specifically stating that it generates "random, unique strings" instead of just "unique strings".

### Supporting reporting of analytics to creators of short URLs who have not registered as user
I would personally not add this. I think that this could definitely be easily added to the system by simply removing much of the ResourceOwner concept. However, I think that conceptually and in practice there is no reason for other users to have access to this information. It exposes too much unwanted/unneeded information, so I woukd not include it.


## Notes
1. For [sync 2](#sync-2-see-notes), I understood the instructions "one when shortenings are translated to targets" as a sync that occurs when a user visits the URL and we need to track the data

2. For [sync 3](#sync-31-and-sync-32-see-notes), I wasn't sure how the user examining analytics would be initiated so I did something similar to the generate and register syncs from the Sync Questions section. I know the instructions said 3 syncs but I figured splitting this one would make it more clear. Sync 3.1 basically waits for an examineAnalytics request and then sends a query to check if the user is the owner of the Url. Then sync 3.2 waits for this call to succeed and if it does then it will return the analytics of the URL. These syncs could also be easily combined into one.

3. I chose to seperate the two concepts ResourceCounter and ResourceOwner because I figured that it would be good for modularity for them to be defined as two different concepts with different purposes. ResourceCounter keeps track of information while ResourceOwner is more used for authentication

[back to table of contents](/assignments/pset2/contents.md)
