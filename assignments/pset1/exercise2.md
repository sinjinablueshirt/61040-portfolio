# Exercise 2
[back to table of contents](/assignments/pset1/contents.md)

**concept** PasswordAuthentication

**purpose** limit access to known users

**principle** after a user registers with a username and a password and confirms a token they recieve via email, they can authenticate with that same username and password and be treated each time as the same user

**state** <br>
&nbsp;&nbsp;a set of Users with <br>
&nbsp;&nbsp;&nbsp;&nbsp;a Username String <br>
&nbsp;&nbsp;&nbsp;&nbsp;a Password String <br>
&nbsp;&nbsp;&nbsp;&nbsp;a confirmed Flag <br>
&nbsp;&nbsp;&nbsp;&nbsp;a token Int <br>

**actions** <br>
&nbsp;&nbsp;register (username: String, password: String): (token: Int) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **requires** the username doesn't already exist <br>
&nbsp;&nbsp;&nbsp;&nbsp; **effects** create a new, non-confirmed user with this username and password, create a token for this user return it

&nbsp;&nbsp;authenticate (username: String, password: String): (user: User) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **requires** the username and password correspond to an existing, confirmed user<br>
&nbsp;&nbsp;&nbsp;&nbsp; **effects** returns the user that corresponds to username and password

&nbsp;&nbsp;confirm (username: String, token: Int) <br>
&nbsp;&nbsp;&nbsp;&nbsp; **requires** the username corresponds to an existing, non-confirmed user and the token matches the token paired with the user <br>
&nbsp;&nbsp;&nbsp;&nbsp; **effects** makes the user a confirmed user

## Question Responses

1. see above

2. see above. For the authenticate action, I figured that if the username and password doesn't correspond to a user, then the effects of the action don't go through.

3. An essential invariant to this state is that no two users can have the same username. This is preserved and we can see this by looking at the only action that adds users to the state: register. In it, we require that the username passed in didn't already exist. This preserves the invariant by making sure that no users with duplicate usernames are created.

## Notes



[back to table of contents](/assignments/pset1/contents.md)
