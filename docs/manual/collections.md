---
description: >-
  Fedify provides a generic way to construct and handle collections.
  This section explains how to work with collections in Fedify.
prev:
  text: Sending activities
  link: ./send.md
next:
  text: Object dispatcher
  link: ./object.md
---

Collections
===========

In ActivityPub, a [collection] is a group of objects.  For example, the
followers collection consists of the followers of an actor, and the outbox
collection consists of the activities that an actor has sent.

Fedify provides a generic way to construct and handle collections.  This
section explains how to work with collections in Fedify.

[collection]: https://www.w3.org/TR/activitypub/#collections


Outbox
------

> [!TIP]
> Since the way to construct an outbox collection is the same as the way to
> construct any other collection, the following examples are also applicable to
> constructing other collections.

First, let's see how to construct an [outbox] collection.  An outbox collection
consists of the activities that an actor has sent.  As each collection has its
own URI, the outbox collection has its own URI, too.  The URI of the outbox
collection is determined by the first parameter of
the `Federation.setOutboxDispatcher()` method:

~~~~ typescript
federation
  .setOutboxDispatcher("/users/{handle}/outbox", async (ctx, handle) => {
    // Work with the database to find the activities that the actor has sent.
    // Omitted for brevity.  See the next example for details.
  });
~~~~

Each actor has its own outbox collection, so the URI pattern of the outbox
dispatcher should include the actor's bare `{handle}`.  The URI pattern syntax
follows the [URI Template] specification.

Since the outbox is a collection of activities, the outbox dispatcher should
return an array of activities.  The following example shows how to construct
an outbox collection:

~~~~ typescript
import { Article, Create } from "@fedify/fedify";

federation
  .setOutboxDispatcher("/users/{handle}/outbox", async (ctx, handle) => {
    // Work with the database to find the activities that the actor has sent
    // (the following `getPostsByUserHandle` is a hypothetical function):
    const posts = await getPostsByUserHandle(handle);
    // Turn the posts into `Create` activities:
    const items = posts.map(post =>
      new Create({
        id: new URL(`/posts/${post.id}#activity`, ctx.url),
        actor: ctx.getActorUri(handle),
        object: new Article({
          id: new URL(`/posts/${post.id}`, ctx.url),
          summary: post.title,
          content: post.content,
        }),
      })
    );
    return { items };
  });
~~~~

If you try to access the outbox collection of an actor, the server will respond
with a JSON object that contains the activities that the actor has sent:

~~~~ http
GET /users/alice/outbox HTTP/1.1
Accept: application/activity+json
Host: localhost
~~~~

~~~~ http
HTTP/1.1 200 OK
Content-Type: application/activity+json
Vary: Accept

{
  "@context": "https://www.w3.org/ns/activitystreams",
  "items": [
    {
      "id": "http://localhost/posts/123#activity",
      "type": "Create",
      "actor": "http://localhost/users/alice",
      "object": {
        "id": "http://localhost/posts/123",
        "type": "Article",
        "summary": "Hello, world!",
        "content": "This is the first post."
      }
    },
    // More items...
  ]
}
~~~~

As you can expect, the server responds with the whole activities that the actor
has sent without any pagination.  In the real world, you should implement
pagination for the outbox collection.  In the next section, we'll see how to
implement pagination for a collection.

[outbox]: https://www.w3.org/TR/activitypub/#outbox
[URI Template]: https://datatracker.ietf.org/doc/html/rfc6570

### Page

A collection page is a subset of a collection.  For example, the first page of
the outbox collection is a collection page that contains the first few items of
the outbox collection.  Each page has its own URI which is determined by
a unique cursor, and links to the next and previous pages if they exist.
No random access is allowed for a collection page; you can only access the next
and previous pages.

Fedify abstracts the concept of a collection page as cursor-based pagination.
The cursor is a string that represents the position in the collection.  It can
be either an opaque token or an offset numeric value; the way to interpret it
is up to the server implementation.

If your database system supports cursor-based pagination ([Deno KV], for
example), you can just use the cursor that the database system provides as is.
If your database system supports only offset-based pagination (the most
relational databases), you can use the offset as the cursor.

Although it's omitted in the previous example, there is the third parameter to
a callback that `~Federation.setOutboxDispatcher()` method takes: the cursor.
When the request is for a collection page, the cursor is passed to the callback
as the third parameter.  When the request is for a whole collection, the cursor
is `null` (that the previous example assumes).

Here's an example of how to implement collection pages for the outbox collection
with assuming that the database system supports cursor-based pagination:

~~~~ typescript
federation
  .setOutboxDispatcher("/users/{handle}/outbox", async (ctx, handle, cursor) => {
    // If a whole collection is requested, returns nothing as we prefer
    // collection pages over the whole collection:
    if (cursor == null) return null;
    // Work with the database to find the activities that the actor has sent
    // (the following `getPostsByUserHandle` is a hypothetical function):
    const { posts, nextCursor, last } = await getPostsByUserHandle(handle, {
      cursor,
      limit: 10,
    });
    // Turn the posts into `Create` activities:
    const items = posts.map(post =>
      new Create({
        id: new URL(`/posts/${post.id}#activity`, ctx.url),
        actor: ctx.getActorUri(handle),
        object: new Article({
          id: new URL(`/posts/${post.id}`, ctx.url),
          summary: post.title,
          content: post.content,
        }),
      })
    );
    return {
      items,
      // If `last` is `true`, it means that the current page is the last page:
      nextCursor: last ? null : nextCursor,
    }
  });
~~~~

In the above example, the hypothetical `getPostsByUserHandle()` function returns
the `nextCursor` along with the `items`.  The `nextCursor` represents the
position of the next page, which is provided by the database system.  If the
`last` is `true`, it means that the current page is the last page, so the
`nextCursor` is `null`.

[Deno KV]: https://deno.com/kv

### First cursor

The first cursor is a special cursor that represents the beginning of the
collection.  It's used to initialize a traversal of the collection.  The first
cursor is `null` if the collection is empty.

The value for the first cursor is determined by
`~CollectionCallbackSetters.setFirstCursor()` method:

~~~~ typescript
// The number of items per page:
const window = 10;

federation
  .setOutboxDispatcher("/users/{handle}/outbox", async (ctx, handle, cursor) => {
    if (cursor == null) return null;
    // The following `getPostsByUserHandle` is a hypothetical function:
    const { posts, nextCursor, last } = await getPostsByUserHandle(
      handle,
      cursor === "" ? { limit: window } : { cursor, limit: window }
    );
    // Turn the posts into `Create` activities:
    const items = posts.map(post =>
      new Create({
        id: new URL(`/posts/${post.id}#activity`, ctx.url),
        actor: ctx.getActorUri(handle),
        object: new Article({
          id: new URL(`/posts/${post.id}`, ctx.url),
          summary: post.title,
          content: post.content,
        }),
      })
    );
    return { items, nextCursor: last ? null : nextCursor }
  })
  .setFirstCursor(async (ctx, handle) => {
    // Let's assume that an empty string represents the beginning of the
    // collection:
    return "";  // Note that it's not `null`.
  });
~~~~

In the above example, the first cursor is an empty string.  When the first
cursor is requested, the server queries the database *without any cursor* to
get the first few items of the collection.

Of course, since the first cursor is also an opaque token, you can use any
string as the first cursor.

> [!NOTE]
> The first cursor is an enabler of the pagination.  If you don't set the first
> cursor, the collection is not considered as paginated, and the server will
> respond with the whole collection without any pagination.

### Counter

As the name suggests, the counter is a callback that counts the *total* number
of items in the collection, which is useful for the client to show, for example,
the total number of articles a user has posted.

The following example shows how to implement the counter for the outbox
collection:

~~~~ typescript
federation
  .setOutboxDispatcher("/users/{handle}/outbox", async (ctx, handle, cursor) => {
    // Omitted for brevity.
  })
  .setCounter(async (ctx, handle) => {
    // The following `countPostsByUserHandle` is a hypothetical function:
    return await countPostsByUserHandle(handle);
  });
~~~~

> [!TIP]
> The counter can return either a `number` or a `bigint`.

### Last cursor

The last cursor is a special cursor that represents the end of the collection.
With the last cursor and `prevCursor`, the client can traverse the collection
backwards.

Since not all database systems support backward pagination, the last cursor is
optional.  If you don't set the last cursor, the client can only traverse the
collection forwards, which is fine in most cases.

So, the below example assumes that the database system supports offset-based
pagination, which is easy to implement backward pagination:

~~~~ typescript
// The number of items per page:
const window = 10;

federation
  .setOutboxDispatcher("/users/{handle}/outbox", async (ctx, handle, cursor) => {
    if (cursor == null) return null;
    // Here we use the offset numeric value as the cursor:
    const offset = parseInt(cursor);
    // The following `getPostsByUserHandle` is a hypothetical function:
    const posts = await getPostsByUserHandle(
      handle,
      { offset, limit: window }
    );
    // Turn the posts into `Create` activities:
    const items = posts.map(post =>
      new Create({
        id: new URL(`/posts/${post.id}#activity`, ctx.url),
        actor: ctx.getActorUri(handle),
        object: new Article({
          id: new URL(`/posts/${post.id}`, ctx.url),
          summary: post.title,
          content: post.content,
        }),
      })
    );
    return { items, nextCursor: (offset + window).toString() }
  })
  .setFirstCursor(async (ctx, handle) => "0")
  .setLastCursor(async (ctx, handle) => {
    // The following `countPostsByUserHandle` is a hypothetical function:
    const total = await countPostsByUserHandle(handle);
    // The last cursor is the offset of the last page:
    return (total - (total % window)).toString();
  });
~~~~

### Constructing outbox collection URIs

To construct an outbox collection URI, you can use the `Context.getOutboxUri()`
method.  This method takes the actor's handle and returns the dereferenceable
URI of the outbox collection of the actor.

The following shows how to construct an outbox collection URI of an actor named
`"alice"`:

~~~~ typescript
ctx.getOutboxUri("alice")
~~~~

> [!NOTE]
>
> The `Context.getOutboxUri()` method does not guarantee that the outbox
> collection actually exists.  It only constructs a URI based on the given
> handle, which may respond with `404 Not Found`.  Make sure to check if the
> handle is valid before calling the method.


Inbox
-----

*This API is available since Fedify 0.11.0.*

The inbox collection is similar to the outbox collection, but it's a collection
of activities that an actor has received.

Cursors and counters for the inbox collection are implemented in the same way as
the outbox collection, so we don't repeat the explanation here.

The below example shows how to construct an inbox collection:

~~~~ typescript
import { Activity } from "@fedify/fedify";

federation
  .setInboxDispatcher("/users/{handle}/inbox", async (ctx, handle) => {
    // Work with the database to find the activities that the actor has received
    // (the following `getInboxByUserHandle` is a hypothetical function):
    const items: Activity[] = await getInboxByUserHandle(handle);
    return { items };
  })
  .setCounter(async (ctx, handle) => {
    // The following `countInboxByUserHandle` is a hypothetical function:
    return await countInboxByUserHandle(handle);
  });
~~~~

> [!NOTE]
> The path for the inbox collection dispatcher must match the path for the inbox
> listeners.

### Constructing inbox collection URIs

To construct an inbox collection URI, you can use the `Context.getInboxUri()`
method.  This method takes the actor's handle and returns the dereferenceable
URI of the inbox collection of the actor.

The following shows how to construct an inbox collection URI of an actor named
`"alice"`:

~~~~ typescript
ctx.getInboxUri("alice")
~~~~

> [!NOTE]
>
> The `Context.getInboxUri()` method does not guarantee that the inbox
> collection actually exists.  It only constructs a URI based on the given
> handle, which may respond with `404 Not Found`.  Make sure to check if the
> handle is valid before calling the method.


Following
---------

The following collection consists of the actors that an actor is following.
The following collection is similar to the outbox collection, but it's a
collection of actors instead of activities.  More specifically, the following
collection can consist of `Actor` objects or `URL` objects that represent the
actors.

Cursors and counters for the following collection are implemented in the same
way as the outbox collection, so we don't repeat the explanation here.

The below example shows how to construct a following collection:

~~~~ typescript
federation
  .setFollowingDispatcher("/users/{handle}/following", async (ctx, handle, cursor) => {
    // If a whole collection is requested, returns nothing as we prefer
    // collection pages over the whole collection:
    if (cursor == null) return null;
    // Work with the database to find the actors that the actor is following
    // (the below `getFollowingByUserHandle` is a hypothetical function):
    const { users, nextCursor, last } = await getFollowingByUserHandle(
      handle,
      cursor === "" ? { limit: 10 } : { cursor, limit: 10 }
    );
    // Turn the users into `URL` objects:
    const items = users.map(actor => actor.uri);
    return { items, nextCursor: last ? null : nextCursor }
  })
  // The first cursor is an empty string:
  .setFirstCursor(async (ctx, handle) => "");
~~~~

### Constructing following collection URIs

To construct a following collection URI, you can use
the `Context.getFollowingUri()` method.  This method takes the actor's handle
and returns the dereferenceable URI of the following collection of the actor.

The following shows how to construct a following collection URI of an actor
named `"alice"`:

~~~~ typescript
ctx.getFollowingUri("alice")
~~~~

> [!NOTE]
> The `Context.getFollowingUri()` method does not guarantee that the following
> collection actually exists.  It only constructs a URI based on the given
> handle, which may respond with `404 Not Found`.  Make sure to check if the
> handle is valid before calling the method.


Followers
---------

The followers collection is very similar to the following collection, but it's
a collection of actors that are following the actor.  The followers collection
has to consist of `Recipient` objects that represent the actors.

The below example shows how to construct a followers collection:

~~~~ typescript
federation
  .setFollowersDispatcher(
    "/users/{handle}/followers",
    async (ctx, handle, cursor) => {
      // If a whole collection is requested, returns nothing as we prefer
      // collection pages over the whole collection:
      if (cursor == null) return null;
      // Work with the database to find the actors that are following the actor
      // (the below `getFollowersByUserHandle` is a hypothetical function):
      const { users, nextCursor, last } = await getFollowersByUserHandle(
        handle,
        cursor === "" ? { limit: 10 } : { cursor, limit: 10 }
      );
      // Turn the users into `Recipient` objects:
      const items = users.map(actor => ({
        id: new URL(actor.uri),
        inboxId: new URL(actor.inboxUri),
      }));
      return { items, nextCursor: last ? null : nextCursor };
    }
  )
  // The first cursor is an empty string:
  .setFirstCursor(async (ctx, handle) => "");
~~~~

> [!TIP]
>
> Every `Actor` object is also a `Recipient` object, so you can use the `Actor`
> object as the `Recipient` object.

### Filtering by server

*This API is available since Fedify 0.8.0.*

The followers collection can be filtered by the base URI of the actor URIs.
It can be useful to filter by a remote server to synchronize the followers
collection with it.

> [!TIP]
> However, the filtering is optional, and you can skip it if you don't need
> [followers collection synchronization](./send.md#followers-collection-synchronization).

In order to filter the followers collection by the server, you need to let
your followers collection dispatcher be aware of the fourth argument:
the base URI of the actor URIs to filter in.  The base URI consists of
the protocol, the authority (the host and the port), and the root path of
the actor URIs.  When the base URI is not `null`, the dispatcher should
return only the actors whose URIs start with the base URI.  If the base URI
is `null`, the dispatcher should return all the actors.

The following example shows how to filter the followers collection by the
server:

~~~~ typescript {8-11}
federation
  .setFollowersDispatcher(
    "/users/{handle}/followers",
    async (ctx, handle, cursor, baseUri) => {
      // Work with the database to find the actors that are following the actor
      // (the below `getFollowersByUserHandle` is a hypothetical function):
      let users = await getFollowersByUserHandle(handle);
      // Filter the actors by the base URI:
      if (baseUri != null) {
        users = users.filter(actor => actor.uri.href.startsWith(baseUri.href));
      }
      // Turn the users into `URL` objects:
      const items = users.map(actor => actor.uri);
      return { items };
    }
  );
~~~~

> [!NOTE]
> In the above example, we filter the actors in memory, but in the real
> world, you should filter the actors in the database query to improve the
> performance.

### Constructing followers collection URIs

To construct a followers collection URI, you can use
the `Context.getFollowersUri()` method.  This method takes the actor's handle
and returns the dereferenceable URI of the followers collection of the actor.

The following shows how to construct a followers collection URI of an actor
named `"alice"`:

~~~~ typescript
ctx.getFollowersUri("alice")
~~~~

> [!NOTE]
>
> The `Context.getFollowersUri()` method does not guarantee that the followers
> collection actually exists.  It only constructs a URI based on the given
> handle, which may respond with `404 Not Found`.  Make sure to check if the
> handle is valid before calling the method.


Liked
-----

*This API is available since Fedify 0.15.0.*

The liked collection is a collection of objects that an actor has liked.
The liked collection is similar to the outbox collection, but it's a collection
of `Object`s instead of `Activity` objects.

Cursors and counters for the liked collection are implemented in the same way as
the outbox collection, so we don't repeat the explanation here.

The below example shows how to construct a liked collection:

~~~~ typescript
import type { Object } from "@fedify/fedify";

federation
  .setLikedDispatcher("/users/{handle}/liked", async (ctx, handle, cursor) => {
    // Work with the database to find the objects that the actor has liked
    // (the below `getLikedPostsByUserHandle` is a hypothetical function):
    const items: Object[] = await getLikedByUserHandle(handle);
    return { items };
  });
~~~~

Or you can yield the object URIs instead of the objects:

~~~~ typescript
import type { Object } from "@fedify/fedify";

federation
  .setLikedDispatcher("/users/{handle}/liked", async (ctx, handle, cursor) => {
    // Work with the database to find the objects that the actor has liked
    // (the below `getLikedPostsByUserHandle` is a hypothetical function):
    const objects: Object[] = await getLikedByUserHandle(handle);
    // Turn the objects into `URL` objects:
    const items: URL[] = objects.map(obj => obj.id).filter(id => id != null);
    return { items };
  });
~~~~


### Constructing liked collection URIs

To construct a liked collection URI, you can use the `Context.getLikedUri()`
method.  This method takes the actor's handle and returns the dereferenceable
URI of the liked collection of the actor.

The following shows how to construct a liked collection URI of an actor named
`"alice"`:

~~~~ typescript
ctx.getLikedUri("alice")
~~~~

> [!NOTE]
>
> The `Context.getLikedUri()` method does not guarantee that the liked
> collection actually exists.  It only constructs a URI based on the given
> handle, which may respond with `404 Not Found`.  Make sure to check if the
> handle is valid before calling the method.


Featured
--------

*This API is available since Fedify 0.11.0.*

The featured collection is a collection of objects that an actor has featured
on top of their profile, i.e., pinned statuses.  The featured collection is
similar to the outbox collection, but it's a collection of any ActivityStreams
objects instead of activities.

Cursor and counter for the featured collection are implemented in the same way
as the outbox collection, so we don't repeat the explanation here.

The below example shows how to construct a featured collection:

~~~~ typescript
federation
  .setFeaturedDispatcher("/users/{handle}/featured", async (ctx, handle, cursor) => {
    // Work with the database to find the objects that the actor has featured
    // (the below `getFeaturedPostsByUserHandle` is a hypothetical function):
    const items = await getFeaturedByUserHandle(handle);
    return { items };
  });
~~~~

### Constructing featured collection URIs

To construct a featured collection URI, you can use
the `Context.getFeaturedUri()` method.  This method takes the actor's handle
and returns the dereferenceable URI of the featured collection of the actor.

The following shows how to construct a featured collection URI of an actor named
`"alice"`:

~~~~ typescript
ctx.getFeaturedUri("alice")
~~~~

> [!NOTE]
>
> The `Context.getFeaturedUri()` method does not guarantee that the featured
> collection actually exists.  It only constructs a URI based on the given
> handle, which may respond with `404 Not Found`.  Make sure to check if the
> handle is valid before calling the method.



Featured tags
-------------

*This API is available since Fedify 0.11.0.*

The featured tags collection is a collection of tags that an actor has featured
on top of their profile.  The featured tags collection is similar to the
featured collection, but it's a collection of `Hashtag` objects instead of
any ActivityStreams objects.

Cursor and counter for the featured tags collection are implemented in the same
way as the outbox collection, so we don't repeat the explanation here.

The below example shows how to construct a featured tags collection:

~~~~ typescript
federation
  .setFeaturedTagsDispatcher("/users/{handle}/tags", async (ctx, handle, cursor) => {
    // Work with the database to find the tags that the actor has featured
    // (the below `getFeaturedTagsByUserHandle` is a hypothetical function):
    const hashtags = await getFeaturedTagsByUserHandle(handle);
    const items = hashtags.map(hashtag =>
      new Hashtag({
        href: new URL(`/tags/${encodeURIComponent(hashtag)}`, ctx.url),
        name: `#${hashtag}`,
      })
    );
    return { items };
  });
~~~~

### Constructing featured tags collection URIs

To construct a featured tags collection URI, you can use
the `Context.getFeaturedTagsUri()` method.  This method takes the actor's handle
and returns the dereferenceable URI of the featured tags collection of
the actor.

The following shows how to construct a featured tags collection URI of an actor
named `"alice"`:

~~~~ typescript
ctx.getFeaturedTagsUri("alice")
~~~~

> [!NOTE]
>
> The `Context.getFeaturedTagsUri()` method does not guarantee that the featured
> tags collection actually exists.  It only constructs a URI based on the given
> handle, which may respond with `404 Not Found`.  Make sure to check
> if the handle is valid before calling the method.
