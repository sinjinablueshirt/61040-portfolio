# Concept Questions
[back to table of contents](/assignments/pset2/contents.md)

## 1. Contexts
The inclusion of the context in NonceGeneration is important because there may be situations where, within some concept, a string cannot be used twice, but it may be used again if it is within another context of that concept. If we didn't have a context in which previous strings were made, then it severely limits the flexibility of NonceGeneration. Essentially, the use of a context allows us to control when the rule of "no duplicates" will apply.

In a URL shortening app, the context might be the domain of the app (like tinyurl.com). When a user shortens a URL using the app, the result would be saved within this domain so that within the domain no two URLs exist that are the same string. Importantly, this allows other domains to have more freedom over the URLs they generate since they won't have to worry about conflictions with URLs from other domains. For example, tinyurl.com can make a URL named jellyfish while yellkey.com can freely make a URL named jellyfish without conflict.

## 2. Storing Used Strings
NonceGeneration is described in the concept specification to store sets of used strings so that it can be implemented in many ways. This is the idea of representation independence which is a key quality to consider when designing specifications. The purpose of the concept is described as "generate unique strings within a context". A used set of strings is the most generic way to describe the state of this concept so that many implementations may map to this concept. It also allows for specific strings to be removed from the concept.

In a counter implementation, an abstraction function mapping the counter implementation's state to a set of used strings would be like AF(counter) = {i for i in range(counter)}

## 3. Words as Nonces
For the user, this strategy might be good because it helps the URL be easily memorable and communicated. For example, if you wanted someone else to visit a URL, it is easier to say "jellyfish" rather than spelling out something like "A-z-e-G-b-P".

However, a flaw in this design is that it lacks security. One of the benefits to having a random string like anjsoDOFAFKWBNO is that it is difficult for someone to get to the URL if they haven't been given it. On the other hand, using dictionary words can make the URL less secure by being more easily guessable.

There are many ways to modify NonceGeneration to use random dictionary strings instead of random strings. The one that comes to mind is by maintaining within the state a set of unused words rather than a set of used words. At the beginning, this set of unused might be every word of a dictionary. When I "generate" a random word, that word should then be removed from the "unused words" set. If one wanted to, they could also maintain a used string set alongside the unused set to have a larger range of behavior.

[back to table of contents](/assignments/pset2/contents.md)
