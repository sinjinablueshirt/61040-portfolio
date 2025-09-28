# Concept Design
[back to table of contents](/assignments/assignment2/contents.md)

### Concept 1
**concept** UserAuthentication[User]

**purpose** resticting who can access certain user accounts

**principle** once a user registers with a username and password, they can later log into the same user with the same username and password

**state**\
&nbsp;&nbsp;a set of User with\
&nbsp;&nbsp;&nbsp;&nbsp;a username String\
&nbsp;&nbsp;&nbsp;&nbsp;a password String\

**actions**\
&nbsp;&nbsp;register(username: String, password: String): User\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** a user with the same username doesn't already exist\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** creates and saves a new user. Returns the user

&nbsp;&nbsp;login(username: String, password: String): User\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** a user exists that has a username and password that matches the passed in username and password\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** returns the user that has a username and password that matches the passed in username and password

### Concept 2
**concept** TagSearch[Object]

**purpose** to organize and retrieve objects based on associated tags

**principle** when an object is registered, the user can attach tags to that object. They can search for objects that have a specific tag attached.

**state**\
&nbsp;&nbsp;a set of Object with\
&nbsp;&nbsp;&nbsp;&nbsp;a tags set of String

**actions**\
&nbsp;&nbsp;register(object: Object, tags: set of String)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** object is not already registered\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** saves object with tags as its tags set

&nbsp;&nbsp;addTag(object: Object, tag: String)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** object is registered\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** adds tag to object's tag set

&nbsp;&nbsp;removeTag(object: Object, tag: String)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** object is registered and has tag in its tag set\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** removes tag from object's tag set

&nbsp;&nbsp;getTags(object: Object): (tags: set of String)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** object is registered\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** returns the tags set of the object

&nbsp;&nbsp;matchTags(tags: set of String): (objects: set of Object)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** true\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** returns the set of Objects that have AT LEAST ALL the passed in tags in their tags set

### Concept 3
**concept** ResourceOwnership[Resource, User]

**purpose** to designate what users own resources

**principle** users can save resources with a description and be the designated owner of that resource

**state**\
&nbsp;&nbsp;a set of Project with\
&nbsp;&nbsp;&nbsp;&nbsp;an owner User\
&nbsp;&nbsp;&nbsp;&nbsp;a resource Resource\
&nbsp;&nbsp;&nbsp;&nbsp;a description String\

**actions**\
&nbsp;&nbsp;loadResource(resource: Resource, owner: User, description: String): (project: Project)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** true\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** creates and saves a new project with resource, owner, and description. Returns the newly made project

&nbsp;&nbsp;changeProject(project: Project, resource: Resource, description: String)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** true\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** changes project's resource and description to the ones passed in

&nbsp;&nbsp;owned(user: User): (projects: set of Project)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** true\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** returns all projects that user owns

&nbsp;&nbsp;check(user: User, project: Project): (project: Project)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** user is the owner of project\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** project is returned


### Concept 4
**concept** Comment[User, Object, dateTime]

**purpose** to allow users to communicate ideas relating to objects

**principle** once an object is registered, users may add comments to the object.

**state**\
&nbsp;&nbsp;a set of Object\
&nbsp;&nbsp;&nbsp;&nbsp;an owner User\
&nbsp;&nbsp;&nbsp;&nbsp;a set of Comment

&nbsp;&nbsp;a set of Comment with\
&nbsp;&nbsp;&nbsp;&nbsp;a text String\
&nbsp;&nbsp;&nbsp;&nbsp;a commenter User\
&nbsp;&nbsp;&nbsp;&nbsp;a dateTime

**actions**\
&nbsp;&nbsp;register(object: Object, owner: User)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** the object isn't already registered\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** saves the object as being owned by owner with an empty set of comments

&nbsp;&nbsp;addComment(object: Object, commenter: User, text: String, dateTime: DateTime): (comment: Comment)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** the object is registered\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** creates and saves a new comment made by commenter with text made at dateTime under the object. Returns the newly made comment

&nbsp;&nbsp;getComments(object: Object): (comments: set of Comments)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** object is registered\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** returns all comments that the object has
<!--
&nbsp;&nbsp;getRecentComments(object: Object, dateTime: DateTime): (comments: set of Comments)\
&nbsp;&nbsp;&nbsp;&nbsp;**requires** object is registered\
&nbsp;&nbsp;&nbsp;&nbsp;**effects** returns all comments that the object has that were recorded after dateTime -->

## Syncs
**sync** registerUser\
**when** Request.registerUser(username, password)\
**then** UserAuthentication.register(username, password)

**sync** login\
**when** Request.login(username, password)\
**then** UserAuthentication.login(username, password)

**sync** loadUsersMusic\
**when** UserAuthentication.login(): (user)\
**then** ResourceOwnership.owned(user)

**sync** uploadMusic\
**when** Request.upload(user, file, description)\
**then** ResourceOwnership.loadResource(resource: file, owner: user, description)

**sync** registerMusic\
**when**\
&nbsp;&nbsp;Request.upload(user, tags)\
&nbsp;&nbsp;ResourceOwnership.loadResource(): (music)\
**then**\
&nbsp;&nbsp;TagSearch.register(object: music, tags+{"music"})\
&nbsp;&nbsp;Comment.register(object: music, user)

**sync** searchMusic\
**when** Request.search(tags)\
**then** TagSearch.matchTags(tags+{"public", "music"}) [see notes](#additional-notes)

**sync** getMusicDetails\
**when**\
&nbsp;&nbsp;Request.displayMusic(music)\
**then**\
&nbsp;&nbsp;TagSearch.getTags(music)\
&nbsp;&nbsp;Comment.getComments(music)\

**sync** giveFeedback\
**when** Request.comment(music: Music, commenter: User, text: String, dateTime: DateTime)\
**then** Comment.addComment(object: music, commenter, text)

**sync** tagComment\
**when**\
&nbsp;&nbsp;Request.comment(tags)\
&nbsp;&nbsp;Comment.addComment(): (comment)\
**then** TagSearch.register(object: comment, tags+{"comment"})

**sync** getCommentTags\
**when** Comment.getComments(): (comments)\
**then** for each comment in comments:\
&nbsp;&nbsp;TagSearch.getTags(object: comment)

**sync** makeMusicPublic\
**when** Request.makePublic(music)\
**then** TagSearch.addTag(object: music, {"public"})

**sync** checkMusicOwnership\
**when** Request.makePrivate(user, music)\
**then** ResourceOwnership.check(user, resource: music)

**sync** makeMusicPrivate\
**when**\
&nbsp;&nbsp;Request.makePrivate()\
&nbsp;&nbsp;ResourceOwnership.check(): (music)\
**then** TagSearch.removeTag(object: music, {"public"})

**sync** editMusic\
**when** Request.editMusic(music, file, description)\
**then** ResourceOwnership.changeProject(project: music, resource: file, description)

## Note
I created four concepts, UserAuthentication, TagSearch, ResourceOwnership, and Comment, that make up the important functionalities of my app. UserAuthentication is a simple yet important concept because this app needs to handle ownership of music which can only be done by users logging in. TagSearch allows the app to associate tags (which i specified as strings) with music and comments to do things like display musical classifications and filter through them when doing searches. As I state in the additional notes, it also allows users to make music private or public. ResourceOwnership is a concept to manage ownership of music. Though it has similarities with TagSearch and UserAuthentication, I chose to seperate it since they have different purposes. Comment is also a simple yet vital concept. It allows users to give feedback on others' music, and it syncs up with TagSearch to tag the feedback.

## Additional Notes:
1.  One big thing I wanted to point out is that when a user does a search, the search will automatically add "public" and "music" tags to the tags the user passed in. This has the effect of only displaying matches to the users tags AND which are public. This goes well with the last few syncs which allow users to make their music private or public.

2. Additionally, the inclusion of the "music" and "comment" tags amongst the syncs are a way for us to differentiate, within the TagSearch concept, between what tagged objects are music and what tagged objects are comments.

[back to table of contents](/assignments/assignment2/contents.md)
