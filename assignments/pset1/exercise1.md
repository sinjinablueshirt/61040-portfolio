# Exercise 1
[back to table of contents](/assignments/pset1/contents.md)

1. **Invariants**: One invariant is that the count of items cannot be less than zero. Another invariant is that every Purchase should have a Request that requested the specific item that was purchased. The latter invariant is more important to uphold because the whole purpose of this app is to give people a way to request what specific gifts they want. If they end up getting gifts that they didn't request, then there's not much purpose in having the app at all. The purchase action is most affected by it, and according to its action spec, it maintains it by requiring that the passed in registry actually has a request for the purchased item with at least count.

2. **Fixing an Action**: removeItem can potentially break this invariant. If an item was purchased, but then a subsequent removeItem was done for that same item, then we have Purchases for an item with no corresponding Request. A way to fix this might be by not allowing removeItem to remove an item if a purchase for that item has already gone through.

3. **Inferring Behavior**: A registry can be opened and closed repeatedly. There are multiple potential reasons for allowing this behavior. One is that there may be scenarios where a registry owner might not want people to purchase items during certain periods while still keeping track of all previous requests and purchases for when they want to open it up again. An example of this might be for school supplies. The teacher may close the registry during periods where supplies aren't needed and reopen it when they are.

4. **Registry Deletion**: An action to delete a registry would not matter in practice. There isn't really a use for it. If a user wanted to remove certain item requests, then they could do it with the removeItem action. If they wanted to close their registry and prevent others from making more purchases, they can use the close action (and reopen it later when they want). Thus adding a delete action would not matter since it is not necessary.

5. **Queries**: The two most common queries will likely be addItems executed by registry owners and purchase from the givers of gifts.

6. **Hiding Purchases**: I would add support for this by holding a flag (hidePurchases) in the state of each registry. Upon creation of each registry using the create command, the owner should designate if they would like to see purchases. The create command would create a registry with the appropriate value on the flag. This flag would not be able to change status once the registry is created.

7. **Generic Types**: It is preferable to define a concept with generic types since it allows for the concept to be applied in many contexts. Also, if we were to just represent items with names or prices, then we would not have as much flexibility to potentially extend/change the concept in the future as well.

[back to table of contents](/assignments/pset1/contents.md)
