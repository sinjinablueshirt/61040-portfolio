# Exercise 3
[back to table of contents](/assignments/pset1/contents.md)

**concept** PersonalAccessToken[User, Resource]

**purpose** limit access to certain resources to certain users

**principle** After a user creates a token for a certain resource, that token can be used to authenticate to gain  access to that resource

**state** <br>
&nbsp;&nbsp;a tokens set of String with <br>
&nbsp;&nbsp;&nbsp;&nbsp;an owner User <br>
&nbsp;&nbsp;&nbsp;&nbsp;a Resource <br>

**actions** <br>
&nbsp;&nbsp;createToken(user: User, resource: Resource): (token: String) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** user and resource have already been created <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** creates a new token string allowing user to access resource, returns the token

&nbsp;&nbsp;authenticate(user: User, token: String): (resource: Resource) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** the user and token must exist, and user must be the owner of token <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** returns the resource that the token is for

## Question Answer

The main area in which PersonalAccessToken differs from PasswordAuthentication is that they are created to limit access to certain resources rather than limit access to a user's account for an example. It allows for much more specific restrictions. In GitHub's case, it gives whoever uses it access to things like repositories.

I would change the GitHub documentation to more thoroughly explain that tokens are used to access resources, much like a code for a safe. A better way to explain it would be stating that passwords are for users while tokens are for the resource. Also, with passwords, each account has only one password. Resources on the other hand may have many personal access tokens that give access to the same parts of it.

## Notes

I used generic parameters User and Resource to write this concept spec to work in any context (not just GitHub). I was a bit unsure if I should use the entire User or just a username, since things like github only need the username and token to access things (i think).

Also, it is possible to easily add on to this concept the idea of public/private resources similar to GitHubs behavior with public/private repos. However, I didn't include it since it makes this concept more applicable to a wider range of contexts.

[back to table of contents](/assignments/pset1/contents.md)
