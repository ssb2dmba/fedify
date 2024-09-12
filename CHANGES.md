<!-- deno-fmt-ignore-file -->

Fedify changelog
================

Version 0.15.1
--------------

To be released.


Version 0.15.0
--------------

Released on September 11, 2024.

 -  Actors, collections, and objects now can have their URIs that do not consist
    of a WebFinger username, which means actors can change their fediverse
    handles.

     -  Added `ActorCallbackSetters.mapHandle()` method.
     -  Added `ActorHandleMapper` type.

 -  Added `quoteUrl` property to `Article`, `ChatMessage`, `Note`, and
    `Question` classes in Activity Vocabulary API.

     -  Added `Article.quoteUrl` property.
     -  `new Article()` constructor now accepts `quoteUrl` option.
     -  `Article.clone()` method now accepts `quoteUrl` option.
     -  Added `ChatMessage.quoteUrl` property.
     -  `new ChatMessage()` constructor now accepts `quoteUrl` option.
     -  `ChatMessage.clone()` method now accepts `quoteUrl` option.
     -  Added `Note.quoteUrl` property.
     -  `new Note()` constructor now accepts `quoteUrl` option.
     -  `Note.clone()` method now accepts `quoteUrl` option.
     -  Added `Question.quoteUrl` property.
     -  `new Question()` constructor now accepts `quoteUrl` option.
     -  `Question.clone()` method now accepts `quoteUrl` option.

 -  The element type of the liked collection is now `Object` or `URL` instead of
    `Like`.

     -  Changed the type of `Federation.setLikedDispatcher()` method's second
        parameter to `CollectionDispatcher<Object | URL,
        RequestContext<TContextData>, TContextData, void>` (was
        `CollectionDispatcher<Like, RequestContext<TContextData>, TContextData,
        void>`).

 -  Removed `expand` option of `Object.toJsonLd()` method, which was deprecated
    in version 0.14.0.  Use `format: "expand"` option instead.

 -  Added `Context.lookupObject()` method.

 -  Default document loaders now recognize ActivityStream objects in more ways:

     -  Loaders now recognize `alternate` ActivityStreams objects in the `Link`
        header.
     -  Loaders now recognize `alternate` ActivityStreams objects in
        the `<link>`/`<a>` HTML elements.

 -  Added `allowPrivateAddress` option to `CreateFederationOptions` interface.

 -  Fixed a bug where the WebFinger response had had a `subject` property
    with an unmatched URI to the requested resource when a non-`acct:` URI
    was given.

 -  Renamed the short option `-c` for `--compact` of `fedify lookup` command to
    `-C` to avoid conflict with the short option `-c` for `--cache-dir`.

 -  Added `-r`/`--raw` option to `fedify lookup` command to output the raw JSON
    object.


Version 0.14.4
--------------

Released on September 6, 2024.

 -  Fixed a bug of `Object.fromJsonLd()` method where it had thrown
    a `TypeError` when the given JSON-LD object had an `@id` property
    with an empty string.


Version 0.14.3
--------------

Released on September 1, 2024.

 -  Fixed `fedify inbox` command where it had ignored `-a`/`--accept-follow`
    options when no `-f`/`--follow` option was provided.  [[#132]]


Version 0.14.2
--------------

Released on August 30, 2024.

 -  Fixed an incompatibility with Meta's [Threads] where sent activities had not
    been verified by their inbox.  [[#125]]


Version 0.14.1
--------------

Released on August 29, 2024.

 -  Fixed `fedify inbox` command that had not been able to parse activities
    even if they are valid JSON-LD.  [[#126]]

 -  Fixed a bug where the *Compact Activity* tab of `fedify inbox` command's
    web interface had shown an expanded JSON-LD object instead of a compacted
    one.


Version 0.14.0
--------------

Released on August 27, 2024.

 -  Removed the limitation that the `sendActivity({ handle: string },
    "followers", Activity)` overload is only available for `RequestContext`
    but not for `Context`.  Now it is available for both.  [[#115]]

     -  Added `Context.sendActivity({ handle: string }, "followers", Activity)`
        overload.
     -  Added type parameter `TContext` to `CollectionsDispatcher` type's first
        parameter to distinguish between `RequestContext` and `Context`.
     -  The first parameter of `CollectionDispatcher` type became `TContext`
        (was `RequestContext`).
     -  Added type parameter `TContext` to `CollectionsCursor` type's first
        parameter to distinguish between `RequestContext` and `Context`.
     -  The first parameter of `CollectionCursor` type became `TContext`
        (was `RequestContext`).
     -  Added type parameter `TContext` to `CollectionsCallbackSetters` type's
        first parameter to distinguish between `RequestContext` and `Context`.

 -  Added `source` property to `Object` class in Activity Vocabulary API.
    [[#114]]

     -  Added `Object.source` property.
     -  `new Object()` constructor now accepts `source` option.
     -  `Object.clone()` method now accepts `source` option.

 -  Added `Source` class to Activity Vocabulary API.  [[#114]]

 -  Added `aliases` property to `Actor` type in Activity Vocabulary API.

     -  Added `Application.getAliases()` method.
     -  Added `Application.getAlias()` method.
     -  `new Application()` constructor now accepts `alias` option.
     -  `new Application()` constructor now accepts `aliases` option.
     -  `Application.clone()` method now accepts `alias` option.
     -  `Application.clone()` method now accepts `aliases` option.
     -  Added `Group.getAliases()` method.
     -  Added `Group.getAlias()` method.
     -  `new Group()` constructor now accepts `alias` option.
     -  `new Group()` constructor now accepts `aliases` option.
     -  `Group.clone()` method now accepts `alias` option.
     -  `Group.clone()` method now accepts `aliases` option.
     -  Added `Organization.getAliases()` method.
     -  Added `Organization.getAlias()` method.
     -  `new Organization()` constructor now accepts `alias` option.
     -  `new Organization()` constructor now accepts `aliases` option.
     -  `Organization.clone()` method now accepts `alias` option.
     -  `Organization.clone()` method now accepts `aliases` option.
     -  Added `Person.getAliases()` method.
     -  Added `Person.getAlias()` method.
     -  `new Person()` constructor now accepts `alias` option.
     -  `new Person()` constructor now accepts `aliases` option.
     -  `Person.clone()` method now accepts `alias` option.
     -  `Person.clone()` method now accepts `aliases` option.
     -  Added `Service.getAliases()` method.
     -  Added `Service.getAlias()` method.
     -  `new Service()` constructor now accepts `alias` option.
     -  `new Service()` constructor now accepts `aliases` option.
     -  `Service.clone()` method now accepts `alias` option.
     -  `Service.clone()` method now accepts `aliases` option.

 -  Improved the performance of `Object.toJsonLd()` method.

     -  `Object.toJsonLd()` method no longer guarantees that the returned
        JSON-LD object is compacted unless the `format: "compact"` option is
        provided.
     -  Added `format` option to `Object.toJsonLd()` method.
     -  Deprecated `expand` option of `Object.toJsonLd()` method.
        Use `format: "expand"` option instead.
     -  The `context` option of `Object.toJsonLd()` method is now only
        applicable to `format: "compact"`.  Otherwise, it throws
        a `TypeError`.

 -  The `getActorHandle()` function now supports cross-origin WebFinger
    resources.

 -  The `lookupWebFinger()` and `getActorHandle()` functions no more throw
    an error when they fail to reach the WebFinger resource.

 -  Collection dispatchers now set the `id` property of
    the `OrderedCollection`/`OrderedCollectionPage` objects that they return
    to the their canonical URI.

 -  Now `fedify init` generates a default *tsconfig.json* file on Node.js and
    Bun, and fills the *deno.json* file with the default `compilerOptions` on
    Deno.

[#114]: https://github.com/dahlia/fedify/issues/114
[#115]: https://github.com/dahlia/fedify/issues/115


Version 0.13.5
--------------

Released on September 6, 2024.

 -  Fixed a bug of `Object.fromJsonLd()` method where it had thrown
    a `TypeError` when the given JSON-LD object had an `@id` property
    with an empty string.


Version 0.13.4
--------------

Released on September 1, 2024.

 -  Fixed `fedify inbox` command where it had ignored `-a`/`--accept-follow`
    options when no `-f`/`--follow` option was provided.  [[#132]]

[#132]: https://github.com/dahlia/fedify/issues/132


Version 0.13.3
--------------

Released on August 30, 2024.

 -  Fixed an incompatibility with Meta's [Threads] where sent activities had not
    been verified by their inbox.  [[#125]]

[Threads]: https://www.threads.net/
[#125]: https://github.com/dahlia/fedify/issues/125


Version 0.13.2
--------------

Released on August 29, 2024.

 -  Fixed `fedify inbox` command that had not been able to parse activities
    even if they are valid JSON-LD.  [[#126]]

[#126]: https://github.com/dahlia/fedify/issues/126


Version 0.13.1
--------------

Released on August 18, 2024.

 -  Fixed a vulnerability where the `getActorHandle()` function had trusted
    the hostname of WebFinger aliases that had not matched the hostname of the
    actor ID (URI).


Version 0.13.0
--------------

Released on August 7, 2024.

 -  Added `closed` property to `Question` class in Activity Vocabulary API.

     -  Added `Question.closed` property.
     -  `new Question()` constructor now accepts `closed` option.
     -  `Question.clone()` method now accepts `closed` option.

 -  Added `voters` property to `Question` class in Activity Vocabulary API.

     -  Added `Question.voters` property.
     -  `new Question()` constructor now accepts `voters` option.
     -  `Question.clone()` method now accepts `voters` option.

 -  HTTP Signatures verification now can be optionally skipped for the sake of
    testing.  [[#110]]

     -  The type of `CreateFederationOptions.signatureTimeWindow` property
        became `Temporal.DurationLike | false` (was `Temporal.DurationLike`).
     -  The type of `VerifyRequestOptions.timeWindow` property became
        `Temporal.DurationLike | false` (was `Temporal.DurationLike`).
     -  Added `CreateFederationOptions.skipSignatureVerification` property.

 -  Removed the singular actor key pair dispatcher APIs which were deprecated
    in version 0.10.0.

     -  Removed the last parameter of the `ActorDispatcher` callback type.
        Use `Context.getActorKeyPairs()` method instead.
     -  Removed `ActorKeyPairDispatcher` type.  Use `ActorKeyPairsDispatcher`
        type instead.
     -  Removed `ActorCallbackSetters.setKeyPairDispatcher()` method.
        Use `ActorCallbackSetters.setKeyPairsDispatcher()` method instead.
     -  Removed `Context.getActorKey()` method.
        Use `Context.getActorKeyPairs()` method instead.

 -  The `Federation` is no more a class, but an interface, which has been
    planned since version 0.10.0.  [[#69]]

     -  `new Federation()` constructor is removed.  Use `createFederation()`
        function instead.
     -  Removed `Federation.sendActivity()` method.
        Use `Context.sendActivity()` method instead.
     -  Removed `Federation` class.
     -  Added `Federation` interface.
     -  Removed `FederationParameters` interface.

 -  Added `fedify tunnel` command to expose a local HTTP server to the public
    internet.

 -  A scaffold project generated by the `fedify init` command has several
    changes:

     -  Added support for [Express] framework.
     -  Added support for [Nitro] framework.
     -  Now a scaffold project uses a [x-forwarded-fetch] middleware to
        support `X-Forwarded-Proto` and `X-Forwarded-Host` headers.
     -  Now a scaffold project has hot reloading by default.
     -  Now a scaffold project has logging configuration using the [LogTape]
        library.

 -  Added more log messages using the [LogTape] library.  Currently the below
    logger categories are used:

     -  `["fedify", "webfinger", "server"]`

[#69]: https://github.com/dahlia/fedify/issues/69
[#110]: https://github.com/dahlia/fedify/issues/110
[Express]: https://expressjs.com/
[Nitro]: https://nitro.unjs.io/


Version 0.12.3
--------------

Released on August 18, 2024.

 -  Fixed a vulnerability where the `getActorHandle()` function had trusted
    the hostname of WebFinger aliases that had not matched the hostname of the
    actor ID (URI).


Version 0.12.2
--------------

Released on July 31, 2024.

 -  Fixed a bug where incoming activities had not been enqueued even
    if the `queue` option was provided to the `createFederation()` function.


Version 0.12.1
--------------

Released on July 27, 2024.

 -  Fixed a bug where `fedify init -w hono` had generated scaffold files without
    Fedify integration.
 -  Fixed a bug where `fedify init -r bun -w hono` had generated scaffold files
    with a wrong port number (was 3000).


Version 0.12.0
--------------

Released on July 24, 2024.

 -  The `fedify` command is now [available on npm][@fedify/cli].  [[#104]]

 -  Incoming activities are now queued before being dispatched to the inbox
    listener if the `queue` option is provided to the `createFederation()`
    function.  [[#70]]

     -  The type of `InboxListener` callback type's first parameter became
        `Context` (was `RequestContext`).
     -  The type of `InboxErrorHandler` callback type's first parameter became
        `Context` (was `RequestContext`).
     -  The type of `SharedInboxKeyDispatcher` callback type's first parameter
        became `Context` (was `RequestContext`).

 -  Implemented fully customizable retry policy for failed tasks in the task
    queue.  By default, the task queue retries the failed tasks with
    an exponential backoff policy with decorrelated jitter.

     -  Added `outboxRetryPolicy` option to `CreateFederationOptions` interface.
     -  Added `inboxRetryPolicy` option to `CreateFederationOptions` interface.
        [[#70]]
     -  Added `RetryPolicy` callback type.
     -  Added `RetryContext` interface.
     -  Added `createExponentialBackoffPolicy()` function.
     -  Added `CreateExponentialBackoffPolicyOptions` interface.

 -  `Federation` object now allows its task queue to be started manually.
    [[#53]]

     -  Added `manuallyStartQueue` option to `CreateFederationOptions`
        interface.
     -  Added `Federation.startQueue()` method.

 -  Made the router able to be insensitive to trailing slashes in the URL paths.
    [[#81]]

     -  Added `trailingSlashInsensitive` option to `CreateFederationOptions`
        interface.
     -  Added `RouterOptions` interface.
     -  Added an optional parameter to `new Router()` constructor.

 -  Added `ChatMessage` class to Activity Vocabulary API.  [[#85]]

 -  Added `Move` class to Activity Vocabulary API.  [[#65], [#92] by Lee Dogeon]

 -  Added `Read` class to Activity Vocabulary API.  [[#65], [#92] by Lee Dogeon]

 -  Added `Travel` class to Activity Vocabulary API.
    [[#65], [#92] by Lee Dogeon]

 -  Added `View` class to Activity Vocabulary API.  [[#65], [#92] by Lee Dogeon]

 -  Added `TentativeAccept` class to Activity Vocabulary API.
    [[#65], [#92] by Lee Dogeon]

 -  Added `TentativeReject` class to Activity Vocabulary API.
    [[#65], [#92] by Lee Dogeon]

 -  Improved multitenancy (virtual hosting) support.  [[#66]]

     -  Added `Context.hostname` property.
     -  Added `Context.host` property.
     -  Added `Context.origin` property.
     -  The type of `ActorKeyPairsDispatcher<TContextData>`'s first parameter
        became `Context` (was `TContextData`).

 -  During verifying HTTP Signatures and Object Integrity Proofs, once fetched
    public keys are now cached.  [[#107]]

     -  The `verifyRequest()` function now caches the fetched public keys
        when the `keyCache` option is provided.
     -  The `verifyProof()` function now caches the fetched public keys
        when the `keyCache` option is provided.
     -  The `verifyObject()` function now caches the fetched public keys
        when the `keyCache` option is provided.
     -  Added `KeyCache` interface.
     -  Added `VerifyRequestOptions.keyCache` property.
     -  Added `VerifyProofOptions.keyCache` property.
     -  Added `VerifyObjectOptions.keyCache` property.
     -  Added `FederationKvPrefixes.publicKey` property.

 -  The built-in document loaders now recognize JSON-LD context provided in
    an HTTP `Link` header. [[#6]]

     -  The `fetchDocumentLoader()` function now recognizes the `Link` header
        with the `http://www.w3.org/ns/json-ld#context` link relation.
     -  The `getAuthenticatedDocumentLoader()` function now returns a document
        loader that recognizes the `Link` header with
        the `http://www.w3.org/ns/json-ld#context` link relation.

 -  Deprecated `Federation.sendActivity()` method.  Use `Context.sendActivity()`
    method instead.

 -  The last parameter of `Federation.sendActivity()` method is no longer
    optional.  Also, it now takes the required `contextData` option.

 -  Removed `Context.getHandleFromActorUri()` method which was deprecated in
    version 0.9.0.  Use `Context.parseUri()` method instead.

 -  Removed `@fedify/fedify/httpsig` module which was deprecated in version
    0.9.0.  Use `@fedify/fedify/sig` module instead.

     -  Removed `sign()` function.
     -  Removed `verify()` function.
     -  Removed `VerifyOptions` interface.

 -  Fixed a bug where the `lookupWebFinger()` function had incorrectly queried
    if the given `resource` was a URL starts with `http:` or had a non-default
    port number.

 -  Fixed a SSRF vulnerability in the built-in document loader.
    [[CVE-2024-39687]]

     -  The `fetchDocumentLoader()` function now throws an error when the given
        URL is not an HTTP or HTTPS URL or refers to a private network address.
     -  Added an optional second parameter to the `fetchDocumentLoader()`
        function, which can be used to allow fetching private network addresses.
     -  The `getAuthenticatedDocumentLoader()` function now returns a document
        loader that throws an error when the given URL is not an HTTP or HTTPS
        URL or refers to a private network address.
     -  Added an optional second parameter to
        the `getAuthenticatedDocumentLoader()` function, which can be used to
        allow fetching private network addresses.

 -  Added `fedify init` subcommand.  [[#105]]

 -  Added more log messages using the [LogTape] library.  Currently the below
    logger categories are used:

     -  `["fedify", "federation", "queue"]`

[@fedify/cli]: https://www.npmjs.com/package/@fedify/cli
[#6]: https://github.com/dahlia/fedify/issues/6
[#53]: https://github.com/dahlia/fedify/issues/53
[#66]: https://github.com/dahlia/fedify/issues/66
[#70]: https://github.com/dahlia/fedify/issues/70
[#81]: https://github.com/dahlia/fedify/issues/81
[#85]: https://github.com/dahlia/fedify/issues/85
[#92]: https://github.com/dahlia/fedify/pull/92
[#104]: https://github.com/dahlia/fedify/issues/104
[#105]: https://github.com/dahlia/fedify/issues/105
[#107]: https://github.com/dahlia/fedify/issues/107


Version 0.11.3
--------------

Released on July 15, 2024.

 -  Fixed a bug where use of `Federation.setInboxDispatcher()` after
    `Federation.setInboxListeners()` had caused a `RouterError` to be
    thrown even if the paths match.  [[#101] by Fabien O'Carroll]

[#101]: https://github.com/dahlia/fedify/pull/101


Version 0.11.2
--------------

Released on July 9, 2024.

 -  Fixed a vulnerability of SSRF via DNS rebinding in the built-in document
    loader.  [[CVE-2024-39687]]

     -  The `fetchDocumentLoader()` function now throws an error when the given
        domain name has any records referring to a private network address.
     -  The `getAuthenticatedDocumentLoader()` function now returns a document
        loader that throws an error when the given domain name has any records
        referring to a private network address.


Version 0.11.1
--------------

Released on July 5, 2024.

 -  Fixed a SSRF vulnerability in the built-in document loader.
    [[CVE-2024-39687]]

     -  The `fetchDocumentLoader()` function now throws an error when the given
        URL is not an HTTP or HTTPS URL or refers to a private network address.
     -  The `getAuthenticatedDocumentLoader()` function now returns a document
        loader that throws an error when the given URL is not an HTTP or HTTPS
        URL or refers to a private network address.


Version 0.11.0
--------------

Released on June 29, 2024.

 -  Improved runtime type error messages for Activity Vocabulary API.  [[#79]]

 -  Added `suppressError` option to dereferencing accessors of Activity
    Vocabulary classes.

 -  Added more collection dispatchers.  [[#78]]

     -  Added `Federation.setInboxDispatcher()` method.  [[#71]]
     -  Added `Federation.setLikedDispatcher()` method.
     -  Added `Context.getLikedUri()` method.
     -  Added `{ type: "liked"; handle: string }` case to `ParseUriResult` type.
     -  Renamed `linked` property (which was a typo) to `liked` in
        `Application`, `Group`, `Organization`, `Person`, and `Service` classes.
     -  Added `Federation.setFeaturedDispatcher()` method.
     -  Added `Context.getFeaturedUri()` method.
     -  Added `{ type: "featured"; handle: string }` case to `ParseUriResult`
        type.
     -  Added `Federation.setFeaturedTagsDispatcher()` method.
     -  Added `Context.getFeaturedTagsUri()` method.
     -  Added `{ type: "featuredTags"; handle: string }` case to
        `ParseUriResult` type.

 -  Frequently used JSON-LD contexts are now preloaded.  [[#74]]

     -  The `fetchDocumentLoader()` function now preloads the following JSON-LD
        contexts:

         -  <https://www.w3.org/ns/activitystreams>
         -  <https://w3id.org/security/v1>
         -  <https://w3id.org/security/data-integrity/v1>
         -  <https://www.w3.org/ns/did/v1>
         -  <https://w3id.org/security/multikey/v1>

     -  The default `rules` for `kvCache()` function are now 5 minutes for all
        URLs.

 -  Added `Invite` class to Activity Vocabulary API.
    [[#65], [#80] by Randy Wressell]

 -  Added `Join` class to Activity Vocabulary API.
    [[#65], [#80] by Randy Wressell]

 -  Added `Leave` class to Activity Vocabulary API.
    [[#65], [#80] by Randy Wressell]

 -  Added `Listen` class to Activity Vocabulary API.
    [[#65], [#80] by Randy Wressell]

 -  Added `Offer` class to Activity Vocabulary API.
    [[#65], [#76] by Lee Dogeon]

 -  The below properties of `Collection` and `CollectionPage` in Activity
    Vocabulary API now do not accept `Link` objects:

     -  `Collection.current`
     -  `Collection.first`
     -  `Collection.last`
     -  `CollectionPage.partOf`
     -  `CollectionPage.next`
     -  `CollectionPage.prev`

 -  Added `featured` property to `Actor` types in Activity Vocabulary API.
    [[#78]]

     -  Added `Application.getFeatured()` method.
     -  Added `Application.featuredId` property.
     -  `new Application()` constructor now accepts `featured` option.
     -  `Application.clone()` method now accepts `featured` option.
     -  Added `Group.getFeatured()` method.
     -  Added `Group.featuredId` property.
     -  `new Group()` constructor now accepts `featured` option.
     -  `Group.clone()` method now accepts `featured` option.
     -  Added `Organization.getFeatured()` method.
     -  Added `Organization.featuredId` property.
     -  `new Organization()` constructor now accepts `featured` option.
     -  `Organization.clone()` method now accepts `featured` option.
     -  Added `Person.getFeatured()` method.
     -  Added `Person.featuredId` property.
     -  `new Person()` constructor now accepts `featured` option.
     -  `Person.clone()` method now accepts `featured` option.
     -  Added `Service.getFeatured()` method.
     -  Added `Service.featuredId` property.
     -  `new Service()` constructor now accepts `featured` option.
     -  `Service.clone()` method now accepts `featured` option.

 -  Added `featuredTags` property to `Actor` types in Activity Vocabulary API.
    [[#78]]

     -  Added `Application.getFeaturedTags()` method.
     -  Added `Application.featuredTagsId` property.
     -  `new Application()` constructor now accepts `featuredTags` option.
     -  `Application.clone()` method now accepts `featuredTags` option.
     -  Added `Group.getFeaturedTags()` method.
     -  Added `Group.featuredTagsId` property.
     -  `new Group()` constructor now accepts `featuredTags` option.
     -  `Group.clone()` method now accepts `featuredTags` option.
     -  Added `Organization.getFeaturedTags()` method.
     -  Added `Organization.featuredTagsId` property.
     -  `new Organization()` constructor now accepts `featuredTags` option.
     -  `Organization.clone()` method now accepts `featuredTags` option.
     -  Added `Person.getFeaturedTags()` method.
     -  Added `Person.featuredTagsId` property.
     -  `new Person()` constructor now accepts `featuredTags` option.
     -  `Person.clone()` method now accepts `featuredTags` option.
     -  Added `Service.getFeaturedTags()` method.
     -  Added `Service.featuredTagsId` property.
     -  `new Service()` constructor now accepts `featuredTags` option.
     -  `Service.clone()` method now accepts `featuredTags` option.

 -  Added `target` property to `Activity` class in Activity Vocabulary API.

     -  Added `Activity.getTarget()` method.
     -  Added `Activity.getTargets()` method.
     -  Added `Activity.targetId` property.
     -  Added `Activity.targetIds` property.
     -  `new Activity()` constructor now accepts `target` option.
     -  `new Activity()` constructor now accepts `targets` option.
     -  `Activity.clone()` method now accepts `target` option.
     -  `Activity.clone()` method now accepts `targets` option.

 -  Added `result` property to `Activity` class in Activity Vocabulary API.

     -  Added `Activity.getResult()` method.
     -  Added `Activity.getResults()` method.
     -  Added `Activity.resultId` property.
     -  Added `Activity.resultIds` property.
     -  `new Activity()` constructor now accepts `result` option.
     -  `new Activity()` constructor now accepts `results` option.
     -  `Activity.clone()` method now accepts `result` option.
     -  `Activity.clone()` method now accepts `results` option.

 -  Added `origin` property to `Activity` class in Activity Vocabulary API.

     -  Added `Activity.getOrigin()` method.
     -  Added `Activity.getOrigins()` method.
     -  Added `Activity.originId` property.
     -  Added `Activity.originIds` property.
     -  `new Activity()` constructor now accepts `origin` option.
     -  `new Activity()` constructor now accepts `origins` option.
     -  `Activity.clone()` method now accepts `origin` option.
     -  `Activity.clone()` method now accepts `origins` option.

 -  Added `instrument` property to `Activity` class in Activity Vocabulary API.

     -  Added `Activity.getInstrument()` method.
     -  Added `Activity.getInstruments()` method.
     -  Added `Activity.instrumentId` property.
     -  Added `Activity.instrumentIds` property.
     -  `new Activity()` constructor now accepts `instrument` option.
     -  `new Activity()` constructor now accepts `instruments` option.
     -  `Activity.clone()` method now accepts `instrument` option.
     -  `Activity.clone()` method now accepts `instruments` option.

 -  The `items` property of `OrderedCollection` and `OrderedCollectionPage`
    in Activity Vocabulary API is now represented as `orderedItems`
    (was `items`) in JSON-LD.

 -  The key pair or the key pair for signing outgoing HTTP requests made from
    the shared inbox now can be configured.  This improves the compatibility
    with other ActivityPub implementations that require authorized fetches
    (i.e., secure mode).

     -  Added `SharedInboxKeyDispatcher` type.
     -  Renamed `InboxListenerSetter` interface to `InboxListenerSetters`.
     -  Added `InboxListenerSetters.setSharedKeyDispatcher()` method.

 -  Followed up the [change in `eddsa-jcs-2022` specification][eddsa-jcs-2022]
    for Object Integrity Proofs.  [[FEP-8b32], [#54]]

[eddsa-jcs-2022]: https://codeberg.org/fediverse/fep/pulls/338
[#71]: https://github.com/dahlia/fedify/issues/71
[#74]: https://github.com/dahlia/fedify/issues/74
[#76]: https://github.com/dahlia/fedify/pull/76
[#78]: https://github.com/dahlia/fedify/issues/78
[#79]: https://github.com/dahlia/fedify/issues/79
[#80]: https://github.com/dahlia/fedify/pull/80


Version 0.10.2
--------------

Released on July 9, 2024.

 -  Fixed a vulnerability of SSRF via DNS rebinding in the built-in document
    loader.  [[CVE-2024-39687]]

     -  The `fetchDocumentLoader()` function now throws an error when the given
        domain name has any records referring to a private network address.
     -  The `getAuthenticatedDocumentLoader()` function now returns a document
        loader that throws an error when the given domain name has any records
        referring to a private network address.


Version 0.10.1
--------------

Released on July 5, 2024.

 -  Fixed a SSRF vulnerability in the built-in document loader.
    [[CVE-2024-39687]]

     -  The `fetchDocumentLoader()` function now throws an error when the given
        URL is not an HTTP or HTTPS URL or refers to a private network address.
     -  The `getAuthenticatedDocumentLoader()` function now returns a document
        loader that throws an error when the given URL is not an HTTP or HTTPS
        URL or refers to a private network address.


Version 0.10.0
--------------

Released on June 18, 2024.

Starting with this release, Fedify, previously distributed under [AGPL 3.0],
is now distributed under the [MIT License] to encourage wider adoption.

 -  Besides RSA-PKCS#1-v1.5, Fedify now supports Ed25519 for signing and
    verifying the activities.  [[#55]]

     -  Added an optional parameter to `generateCryptoKeyPair()` function,
        `algorithm`, which can be either `"RSASSA-PKCS1-v1_5"` or `"Ed25519"`.
     -  The `importJwk()` function now accepts Ed25519 keys.
     -  The `exportJwk()` function now exports Ed25519 keys.
     -  The `importSpki()` function now accepts Ed25519 keys.
     -  The `exportJwk()` function now exports Ed25519 keys.

 -  Now multiple key pairs can be registered for an actor.  [[FEP-521a], [#55]]

     -  Added `Context.getActorKeyPairs()` method.
     -  Deprecated `Context.getActorKey()` method.
        Use `Context.getActorKeyPairs()` method instead.
     -  Added `ActorKeyPair` interface.
     -  Added `ActorCallbackSetters.setKeyPairsDispatcher()` method.
     -  Added `ActorKeyPairsDispatcher` type.
     -  Deprecated `ActorCallbackSetters.setKeyPairDispatcher()` method.
     -  Deprecated `ActorKeyPairDispatcher` type.
     -  Deprecated the third parameter of the `ActorDispatcher` callback type.
        Use `Context.getActorKeyPairs()` method instead.

 -  Added `Multikey` class to Activity Vocabulary API.  [[FEP-521a], [#55]]

     -  Added `importMultibaseKey()` function.
     -  Added `exportMultibaseKey()` function.

 -  Added `assertionMethod` property to the `Actor` types in the Activity
    Vocabulary API.  [[FEP-521a], [#55]]

     -  Added `Application.getAssertionMethod()` method.
     -  Added `Application.getAssertionMethods()` method.
     -  `new Application()` constructor now accepts `assertionMethod` option.
     -  `new Application()` constructor now accepts `assertionMethods` option.
     -  `Application.clone()` method now accepts `assertionMethod` option.
     -  `Application.clone()` method now accepts `assertionMethods` option.
     -  Added `Group.getAssertionMethod()` method.
     -  Added `Group.getAssertionMethods()` method.
     -  `new Group()` constructor now accepts `assertionMethod` option.
     -  `new Group()` constructor now accepts `assertionMethods` option.
     -  `Group.clone()` method now accepts `assertionMethod` option.
     -  `Group.clone()` method now accepts `assertionMethods` option.
     -  Added `Organization.getAssertionMethod()` method.
     -  Added `Organization.getAssertionMethods()` method.
     -  `new Organization()` constructor now accepts `assertionMethod` option.
     -  `new Organization()` constructor now accepts `assertionMethods` option.
     -  `Organization.clone()` method now accepts `assertionMethod` option.
     -  `Organization.clone()` method now accepts `assertionMethods` option.
     -  Added `Person.getAssertionMethod()` method.
     -  Added `Person.getAssertionMethods()` method.
     -  `new Person()` constructor now accepts `assertionMethod` option.
     -  `new Person()` constructor now accepts `assertionMethods` option.
     -  `Person.clone()` method now accepts `assertionMethod` option.
     -  `Person.clone()` method now accepts `assertionMethods` option.
     -  Added `Service.getAssertionMethod()` method.
     -  Added `Service.getAssertionMethods()` method.
     -  `new Service()` constructor now accepts `assertionMethod` option.
     -  `new Service()` constructor now accepts `assertionMethods` option.
     -  `Service.clone()` method now accepts `assertionMethod` option.
     -  `Service.clone()` method now accepts `assertionMethods` option.

 -  Added `DataIntegrityProof` class to Activity Vocabulary API.
    [[FEP-8b32], [#54]]

 -  Added `proof` property to the `Object` class in the Activity
    Vocabulary API.  [[FEP-8b32], [#54]]

     -  Added `Object.getProof()` method.
     -  Added `Object.getProofs()` method.
     -  `new Object()` constructor now accepts `proof` option.
     -  `new Object()` constructor now accepts `proofs` option.
     -  `Object.clone()` method now accepts `proof` option.
     -  `Object.clone()` method now accepts `proofs` option.

 -  Implemented Object Integrity Proofs.  [[FEP-8b32], [#54]]

     -  If there are any Ed25519 key pairs, the `Context.sendActivity()` and
        `Federation.sendActivity()` methods now make Object Integrity Proofs
        for the activity to be sent.
     -  If the incoming activity has Object Integrity Proofs, the inbox listener
        now verifies them and ignores HTTP Signatures (if any).
     -  Added `signObject()` function.
     -  Added `SignObjectOptions` interface.
     -  Added `createProof()` function.
     -  Added `CreateProofOptions` interface.
     -  Added `verifyObject()` function.
     -  Added `VerifyObjectOptions` interface.
     -  Added `verifyProof()` function.
     -  Added `VerifyProofOptions` interface.
     -  Added `fetchKey()` function.
     -  Added `FetchKeyOptions` interface.
     -  Added `SenderKeyPair` interface.
     -  The type of `Federation.sendActivity()` method's first parameter became
        `SenderKeyPair[]` (was `{ keyId: URL; privateKey: CryptoKey }`).
     -  The `Context.sendActivity()` method's first parameter now accepts
        `SenderKeyPair[]` as well.

 -  In the future, `Federation` class will become an interface.
    For the forward compatibility, the following changes are made:

     -  Added `createFederation()` function.
     -  Added `CreateFederationOptions` interface.
     -  Deprecated `new Federation()` constructor.  Use `createFederation()`
        function instead.
     -  Deprecated `FederationParameters` interface.

 -  Added `Arrive` class to Activity Vocabulary API.
    [[#65], [#68] by Randy Wressell]

 -  Added `Question` class to Activity Vocabulary API.

 -  Added `context` option to `Object.toJsonLd()` method.  This applies to
    any subclasses of the `Object` class too.

 -  Deprecated `treatHttps` option in `FederationParameters` interface.
    Instead, use the [x-forwarded-fetch] library to recognize the
    `X-Forwarded-Host` and `X-Forwarded-Proto` headers.

 -  Removed the `Federation.handle()` method which was deprecated in version
    0.6.0.

 -  Removed the `integrateHandlerOptions()` function from
    `@fedify/fedify/x/fresh` which was deprecated in version 0.6.0.

 -  Ephemeral actors and inboxes that the `fedify inbox` command spawns are
    now more interoperable with other ActivityPub implementations.

     -  Ephemeral actors now have the following properties: `summary`,
        `following`, `followers`, `outbox`, `manuallyApprovesFollowers`, and
        `url`.
     -  Improved the compatibility of the `fedify inbox` command with Misskey
        and Mitra.

 -  Added more log messages using the [LogTape] library.  Currently the below
    logger categories are used:

     -  `["fedify", "sig", "proof"]`
     -  `["fedify", "sig", "key"]`
     -  `["fedify", "vocab", "lookup"]`
     -  `["fedify", "webfinger", "lookup"]`

[#54]: https://github.com/dahlia/fedify/issues/54
[#55]: https://github.com/dahlia/fedify/issues/55
[#65]: https://github.com/dahlia/fedify/issues/65
[#68]: https://github.com/dahlia/fedify/pull/68
[AGPL 3.0]: https://www.gnu.org/licenses/agpl-3.0.en.html
[MIT License]: https://minhee.mit-license.org/
[FEP-521a]: https://w3id.org/fep/521a
[FEP-8b32]: https://w3id.org/fep/8b32
[x-forwarded-fetch]: https://github.com/dahlia/x-forwarded-fetch


Version 0.9.3
-------------

Released on July 9, 2024.

 -  Fixed a vulnerability of SSRF via DNS rebinding in the built-in document
    loader.  [[CVE-2024-39687]]

     -  The `fetchDocumentLoader()` function now throws an error when the given
        domain name has any records referring to a private network address.
     -  The `getAuthenticatedDocumentLoader()` function now returns a document
        loader that throws an error when the given domain name has any records
        referring to a private network address.


Version 0.9.2
-------------

Released on July 5, 2024.

 -  Fixed a SSRF vulnerability in the built-in document loader.
    [[CVE-2024-39687]]

     -  The `fetchDocumentLoader()` function now throws an error when the given
        URL is not an HTTP or HTTPS URL or refers to a private network address.
     -  The `getAuthenticatedDocumentLoader()` function now returns a document
        loader that throws an error when the given URL is not an HTTP or HTTPS
        URL or refers to a private network address.

[CVE-2024-39687]: https://github.com/dahlia/fedify/security/advisories/GHSA-p9cg-vqcc-grcx


Version 0.9.1
-------------

Released on June 13, 2024.

 -  Fixed a bug of Activity Vocabulary API that `clone()` method of Vocabulary
    classes had not cloned the `id` property from the source object.


Version 0.9.0
-------------

Released on June 2, 2024.

 -  Added `Tombstone` class to Activity Vocabulary API.

 -  Added `Hashtag` class to Activity Vocabulary API.  [[#48]]

 -  Added `Emoji` class to Activity Vocabulary API.  [[#48]]

 -  Added an actor handle normalization function.

     -  Added `normalizeActorHandle()` function.
     -  Added `NormalizeActorHandleOptions` interface.
     -  The `getActorHandle()` function now guarantees that the returned
        actor handle is normalized.
     -  Added the second optional parameter to `getActorHandle()` function.
     -  The return type of `getActorHandle()` function became
        ``Promise<`@${string}@${string}` | `${string}@${string}`>``
        (was ``Promise<`@${string}@${string}`>``).

 -  Added `excludeBaseUris` option to `Context.sendActivity()` and
    `Federation.sendActivity()` methods.

     -  Added `SendActivityOptions.excludeBaseUris` property.
     -  Added `ExtractInboxesParameters.excludeBaseUris` property.

 -  The `Context` now can parse URIs of objects, inboxes, and collections as
    well as actors.

     -  Added `Context.parseUri()` method.
     -  Added `ParseUriResult` type.
     -  Deprecated `Context.getHandleFromActorUri()` method.

 -  The time window for signature verification is now configurable.  [[#52]]

     -  The default time window for signature verification is now a minute (was
        30 seconds).
     -  Added `signatureTimeWindow` option to `FederationParameters` interface.
     -  Added `VerifyOptions` interface.
     -  The signature of the `verify()` function is revamped; it now optionally
        takes a `VerifyOptions` object as the second parameter.

 -  Renamed the `@fedify/fedify/httpsig` module to `@fedify/fedify/sig`, and
    also:

     -  Deprecated `sign()` function.  Use `signRequest()` instead.
     -  Deprecated `verify()` function.  Use `verifyRequest()` instead.
     -  Deprecated `VerifyOptions` interface.  Use `VerifyRequestOptions`
        instead.

 -  When signing an HTTP request, the `algorithm` parameter is now added to
    the `Signature` header.  This change improves the compatibility with
    Misskey and other implementations that require the `algorithm` parameter.

 -  Added more log messages using the [LogTape] library.  Currently the below
    logger categories are used:

     -  `["fedify", "federation", "actor"]`
     -  `["fedify", "federation", "http"]`
     -  `["fedify", "sig", "http"]`
     -  `["fedify", "sig", "key"]`
     -  `["fedify", "sig", "owner"]`

[#48]: https://github.com/dahlia/fedify/issues/48
[#52]: https://github.com/dahlia/fedify/issues/52


Version 0.8.0
-------------

Released on May 6, 2024.

 -  The CLI toolchain for testing and debugging is now available on JSR:
    [@fedify/cli].  You can install it with
    `deno install -A --unstable-fs --unstable-kv --unstable-temporal -n fedify
    jsr:@fedify/cli`, or download a standalone executable from the [releases]
    page.

     -  Added `fedify` command.
     -  Added `fedify lookup` subcommand.
     -  Added `fedify inbox` subcommand.

 -  Implemented [followers collection synchronization mechanism][FEP-8fcf].

     -  Added `RequestContext.sendActivity()` overload that takes `"followers"`
        as the second parameter.
     -  Added the second type parameter to `CollectionCallbackSetters`
        interface.
     -  Added the second type parameter to `CollectionDispatcher` type.
     -  Added the fourth parameter to `CollectionDispatcher` type.
     -  Added the second type parameter to `CollectionCounter` type.
     -  Added the third parameter to `CollectionCounter` type.
     -  Added the second type parameter to `CollectionCursor` type.
     -  Added the third parameter to `CollectionCursor` type.

 -  Relaxed the required type for activity recipients.

     -  Added `Recipient` interface.
     -  The type of the second parameter of `Context.sendActivity()` method
        became `Recipient | Recipient[]` (was `Actor | Actor[]`).  However,
        since `Recipient` is a supertype of `Actor`, the existing code should
        work without any change.

 -  Followers collection now has to consist of `Recipient` objects only.
    (It could consist of `URL`s as well as `Actor`s before.)

     -  The type of `Federation.setFollowersDispatcher()` method's second
        parameter became `CollectionDispatcher<Recipient, TContextData, URL>`
        (was `CollectionDispatcher<Actor | URL, TContextData>`).

 -  Some of the responsibility of a document loader was separated to a context
    loader and a document loader.

     -  Added `contextLoader` option to constructors, `fromJsonLd()` static
        methods, `clone()` methods, and all non-scalar accessors (`get*()`) of
        Activity Vocabulary classes.
     -  Renamed `documentLoader` option to `contextLoader` in `toJsonLd()`
        methods of Activity Vocabulary objects.
     -  Added `contextLoader` option to `LookupObjectOptions` interface.
     -  Added `contextLoader` property to `Context` interface.
     -  Added `contextLoader` option to `FederationParameters` interface.
     -  Renamed `documentLoader` option to `contextLoader` in
        `RespondWithObjectOptions` interface.
     -  Added `GetKeyOwnerOptions` interface.
     -  The type of the second parameter of `getKeyOwner()` function became
        `GetKeyOwnerOptions` (was `DocumentLoader`).
     -  Added `DoesActorOwnKeyOptions` interface.
     -  The type of the third parameter of `doesActorOwnKey()` function became
        `DoesActorOwnKeyOptions` (was `DocumentLoader`).

 -  Added `width` and `height` properties to `Document` class for better
    compatibility with Mastodon.  [[#47]]

     -  Added `Document.width` property.
     -  Added `Document.height` property.
     -  `new Document()` constructor now accepts `width` option.
     -  `new Document()` constructor now accepts `height` option.
     -  `Document.clone()` method now accepts `width` option.
     -  `Document.clone()` method now accepts `height` option.

 -  Removed the dependency on *@js-temporal/polyfill* on Deno, and Fedify now
    requires `--unstable-temporal` flag.  On other runtime, it still depends
    on *@js-temporal/polyfill*.

 -  Added more log messages using the [LogTape] library.  Currently the below
    logger categories are used:

     -  `["fedify", "federation", "collection"]`
     -  `["fedify", "httpsig", "verify"]`
     -  `["fedify", "runtime", "docloader"]`

 -  Fixed a bug where the authenticated document loader had thrown `InvalidUrl`
    error when the URL redirection was involved in Bun.

 -  Fixed a bug of `lookupObject()` that it had failed to look up the actor
    object when WebFinger response had no links with
    `"type": "application/activity+json"` but had `"type":
    "application/ld+json; profile=\"https://www.w3.org/ns/activitystreams\""`.

[@fedify/cli]: https://jsr.io/@fedify/cli
[releases]: https://github.com/dahlia/fedify/releases
[FEP-8fcf]: https://w3id.org/fep/8fcf
[#47]: https://github.com/dahlia/fedify/issues/47


Version 0.7.0
-------------

Released on April 23, 2024.

 -  Added `PUBLIC_COLLECTION` constant for [public addressing].

 -  `Federation` now supports [authorized fetch] for actor dispatcher and
    collection dispatchers.

     -  Added `ActorCallbackSetters.authorize()` method.
     -  Added `CollectionCallbackSetters.authorize()` method.
     -  Added `AuthorizedPredicate` type.
     -  Added `RequestContext.getSignedKey()` method.
     -  Added `RequestContext.getSignedKeyOwner()` method.
     -  Added `FederationFetchOptions.onUnauthorized` option for handling
        unauthorized fetches.
     -  Added `getKeyOwner()` function.

 -  The default implementation of `FederationFetchOptions.onNotAcceptable`
    option now responds with `Vary: Accept, Signature` header.

 -  Added log messages using the [LogTape] library.  Currently the below
    logger categories are used:

     -  `["fedify"]`
     -  `["fedify", "federation"]`
     -  `["fedify", "federation", "inbox"]`
     -  `["fedify", "federation", "outbox"]`

 -  Added `RequestContext.getActor()` method.

 -  Activity Vocabulary classes now have `typeId` static property.

 -  Dispatcher setters and inbox listener setters in `Federation` now take
    a path as `` `${string}{handle}${string}` `` instead of `string`
    so that it is more type-safe.

 -  Added generalized object dispatchers.  [[#33]]

     -  Added `Federation.setObjectDispatcher()` method.
     -  Added `ObjectDispatcher` type.
     -  Added `ObjectAuthorizePredicate` type.
     -  Added `Context.getObjectUri()` method.
     -  Added `RequestContext.getObject()` method.

[public addressing]: https://www.w3.org/TR/activitypub/#public-addressing
[authorized fetch]: https://swicg.github.io/activitypub-http-signature/#authorized-fetch
[LogTape]: https://github.com/dahlia/logtape
[#33]: https://github.com/dahlia/fedify/issues/33


Version 0.6.1
-------------

Released on April 17, 2024.

 -  Fixed a bug of `new Federation()` constructor that if it is once called
    the process will never exit.  [[#39]]


Version 0.6.0
-------------

Released on April 9, 2024.

 -  `DocumentLoader` is now propagated to the loaded remote objects from
    Activity Vocabulary objects.  [[#27]]

     -  Added `options` parameter to Activity Vocabulary constructors.
     -  Added `options` parameter to `clone()` method of Activity Vocabulary
        objects.
     -  The Activity Vocabulary object passed to `InboxListener` now implicitly
        loads remote object with an authenticated `DocumentLoader`.

 -  Added `Federation.fetch()` method.

     -  Deprecated `Federation.handle()` method.  Use `Federation.fetch()`
        method instead.
     -  Renamed `FederationHandlerParameters` type to `FederationFetchOptions`.
     -  Added `integrateFetchOptions()` function.
     -  Deprecated `integrateHandlerOptions()` function.

 -  Added `@fedify/fedify/x/hono` module for integrating with [Hono] middleware.
    [[#25]]

     -  Added `federation()` function.
     -  Added `ContextDataFactory` type.

 -  `Context.sendActivity()` method now throws `TypeError` instead of silently
    failing when the given `Activity` object lacks the actor property.

 -  `Context.sendActivity()` method now uses an authenticated document
    loader under the hood.

 -  Added outbox error handler to `Federation`.

     -  Added `onOutboxError` option to `new Federation()` constructor.
     -  Added `OutboxErrorHandler` type.

[Hono]: https://hono.dev/
[#25]: https://github.com/dahlia/fedify/issues/25
[#27]: https://github.com/dahlia/fedify/issues/27


Version 0.5.2
-------------

Released on April 17, 2024.

 -  Fixed a bug of `new Federation()` constructor that if it is once called
    the process will never exit.  [[#39]]

[#39]: https://github.com/dahlia/fedify/issues/39


Version 0.5.1
-------------

Released on April 5, 2024.

 -  Fixed a bug of `Federation` that its actor/collection dispatchers had done
    content negotiation before determining if the resource exists or not.
    It also fixed a bug that `integrateHandler()` from `@fedify/fedify/x/fresh`
    had responded with `406 Not Acceptable` instead of `404 Not Found` when
    the resource does not exist in the web browser.  [[#34]]

[#34]: https://github.com/dahlia/fedify/issues/34


Version 0.5.0
-------------

Released on April 2, 2024.

 -  Fedify is now available on npm: [@fedify/fedify].  [[#24]]

 -  Abstract key-value store for caching.

     -  Added `KvStore` interface.
     -  Added `KvStoreSetOptions` interface.
     -  Added `KvKey` type.
     -  Added `DenoKvStore` class.
     -  `KvCacheParameters.kv` option now accepts a `KvStore` instead of
        `Deno.Kv`.
     -  `KvCacheParameters.prefix` option now accepts a `KvKey` instead of
        `Deno.KvKey`.
     -  `FederationParameters.kv` option now accepts a `KvStore` instead of
        `Deno.Kv`.
     -  `FederationKvPrefixes.activityIdempotence` option now accepts a `KvKey`
        instead of `Deno.KvKey`.
     -  `FederationKvPrefixes.remoteDocument` option now accepts a `KvKey`
        instead of `Deno.KvKey`.

 -  Abstract message queue for outgoing activities.

     -  Added `MessageQueue` interface.
     -  Added `MessageQueueEnqueueOptions` interface.
     -  Added `InProcessMessageQueue` class.
     -  Added `FederationParameters.queue` option.

 -  Added `@fedify/fedify/x/denokv` module for adapting `Deno.Kv` to `KvStore`
    and `MessageQueue`.  It is only available in Deno runtime.

     -  Added `DenoKvStore` class.
     -  Added `DenoKvMessageQueue` class.

 -  Added `PropertyValue` to Activity Vocabulary API.  [[#29]]

     -  Added `PropertyValue` class.
     -  `new Object()` constructor's `attachments` option now accepts
        `PropertyValue` objects.
     -  `new Object()` constructor's `attachment` option now accepts
        a `PropertyValue` object.
     -  `Object.getAttachments()` method now yields `PropertyValue` objects
        besides `Object` and `Link` objects.
     -  `Object.getAttachment()` method now returns a `PropertyValue` object
        besides an `Object` and a `Link` object.
     -  `Object.clone()` method's `attachments` option now accepts
        `PropertyValue` objects.
     -  `Object.clone()` method's `attachment` option now accepts
        a `PropertyValue` object.

 -  Removed dependency on *jose*.

     -  Added `exportSpki()` function.
     -  Added `importSpki()` function.

 -  Fixed a bug that `Application.manuallyApprovesFollowers`,
    `Group.manuallyApprovesFollowers`, `Organization.manuallyApprovesFollowers`,
    `Person.manuallyApprovesFollowers`, and `Service.manuallyApprovesFollowers`
    properties were not properly displayed in Mastodon.

[@fedify/fedify]: https://www.npmjs.com/package/@fedify/fedify
[#24]: https://github.com/dahlia/fedify/discussions/24
[#29]: https://github.com/dahlia/fedify/issues/29


Version 0.4.0
-------------

Released on March 26, 2024.

 -  Added `@fedify/fedify/x/fresh` module for integrating with [Fresh]
    middleware.

     -  Added `integrateHandler()` function.
     -  Added `integrateHandlerOptions()` function.

 -  Added `getActorHandle()` function.

 -  Fedify now has authenticated document loader.  [[#12]]

     -  Added `Context.getDocumentLoader()` method.
     -  Added `getAuthenticatedDocumentLoader()` function.
     -  Added `AuthenticatedDocumentLoaderFactory` type.
     -  Added `authenticatedDocumentLoaderFactory` option to `new Federation()`
        constructor.
     -  `Context.documentLoader` property now returns an authenticated document
        loader in personal inbox listeners.  (Note that it's not affected in
        shared inbox listeners.)

 -  Added singular accessors to `Object`'s `icon` and `image` properties.

     -  `new Object()` constructor now accepts `icon` option.
     -  `new Object()` constructor now accepts `image` option.
     -  Added `Object.getIcon()` method.
     -  Added `Object.getImage()` method.
     -  `Object.clone()` method now accepts `icon` option.
     -  `Object.clone()` method now accepts `image` option.

 -  `Object`'s `icon` and `image` properties no more accept `Link` objects.

     -  `new Object()` constructor's `icons` option no more accepts `Link`
        objects.
     -  `new Object()` constructor's `images` option no more accepts `Link`
        objects.
     -  `Object.getIcons()` method no more yields `Link` objects.
     -  `Object.getImages()` method no more yields `Link` objects.
     -  `Object.clone()` method's `icons` option no more accepts `Link` objects.
     -  `Object.clone()` method's `images` option no more accepts `Link`
        objects.

 -  `Object`'s `attributedTo` property was renamed to `attribution`.

    -  `new Object()` constructor's `attributedTo` option was renamed to
       `attribution`.
    -  `new Object()` constructor's `attributedTos` option was renamed to
       `attributions`.
    -  `Object.getAttributedTo()` method is renamed to
       `Object.getAttribution()`.
    -  `Object.getAttributedTos()` method is renamed to
       `Object.getAttributions()`.
    -  `Object.clone()` method's `attributedTo` option is renamed to
       `attribution`.
    -  `Object.clone()` method's `attributedTos` option is renamed to
       `attributions`.

 -  `Object`'s `attribution` property (was `attributedTo`) now accepts only
    `Actor` objects.

     -  `new Object()` constructor's `attribution` option (was `attributedTo`)
        now accepts only an `Actor` object.
     -  `new Object()` constructor's `attributions` option (was `attributedTos`)
        now accepts only `Actor` objects.
     -  `Object.getAttribution()` method (was `getAttributedTo()`) now returns
        only an `Actor` object.
     -  `Object.getAttributions()` method (was `getAttributedTos()`) now returns
        only `Actor` objects.
     -  `Object.clone()` method's `attribution` option (`attributedTo`) now
        accepts only an `Actor` object.
     -  `Object.clone()` method's `attributions` option (`attributedTos`) now
        accepts only `Actor` objects.

 -  `Activity`'s `object` property no more accepts `Link` objects.

     -  `new Activity()` constructor's `object` option no more accepts a `Link`
        object.
     -  `new Activity()` constructor's `objects` option no more accepts `Link`
        objects.
     -  `Activity.getObject()` method no more returns a `Link` object.
     -  `Activity.getObjects()` method no more returns `Link` objects.
     -  `Activity.clone()` method's `object` option no more accepts a `Link`
        object.
     -  `Activity.clone()` method's `objects` option no more accepts `Link`
        objects.

 -  `Activity`'s `actor` property now accepts only `Actor` objects.

     -  `new Activity()` constructor's `actor` option now accepts only
        an `Actor` object.
     -  `new Activity()` constructor's `actors` option now accepts only `Actor`
        objects.
     -  `Activity.getActor()` method now returns only an `Actor` object.
     -  `Activity.getActors()` method now returns only `Actor` objects.
     -  `Activity.clone()` method's `actor` option now accepts only an `Actor`
        object.
     -  `Activity.clone()` method's `actors` option now accepts only `Actor`
        objects.

 -  Added `sensitive` property to `Object` class.

     -  `new Object()` constructor now accepts `sensitive` option.
     -  Added `Object.sensitive` attribute.
     -  `Object.clone()` method now accepts `sensitive` option.

 -  Now `lookupWebFinger()` follows redirections.

 -  The `http://webfinger.net/rel/profile-page` links in WebFinger responses
    now omit `type` property.

[Fresh]: https://fresh.deno.dev/
[#12]: https://github.com/dahlia/fedify/issues/12


Version 0.3.0
-------------

Released on March 15, 2024.

 -  Added utility functions for responding with an ActivityPub object:

     -  Added `respondWithObject()` function.
     -  Added `respondWithObjectIfAcceptable()` function.
     -  Added `RespondWithObjectOptions` interface.

 -  Added utility functions for generating and exporting cryptographic keys
    which are compatible with popular ActivityPub software:

     -  Added `generateCryptoKeyPair()` function.
     -  Added `exportJwk()` function.
     -  Added `importJwk()` function.

 -  The following functions and methods now throw `TypeError` if the specified
    `CryptoKey` is not `extractable`:

     -  `Context.getActorKey()` method
     -  `Context.sendActivity()` method
     -  `Federation.sendActivity()` method

 -  Added `immediate` option to `Context.sendActivity()` and
    `Federation.sendActivity()` methods.

 -  Added `SendActivityOptions` interface.

 -  Now `onNotFound`/`onNotAcceptable` options are optional for
    `Federation.handle()` method.  [[#9]]

[#9]: https://github.com/dahlia/fedify/issues/9


Version 0.2.0
-------------

Released on March 10, 2024.

 -  Implemented [NodeInfo] 2.1 protocol.  [[#1]]

     -  Now `Federation.handle()` accepts requests for */.well-known/nodeinfo*.
     -  Added `Federation.setNodeInfoDispatcher()` method.
     -  Added `Context.getNodeInfoUri()` method.
     -  Added `NodeInfo` interface.
     -  Added `Software` interface.
     -  Added `Protocol` type.
     -  Added `Services` interface.
     -  Added `InboundService` type.
     -  Added `OutboundService` type.
     -  Added `Usage` interface.
     -  Added `NodeInfoDispatcher` type.
     -  Added `nodeInfoToJson()` function.

 -  Implemented [WebFinger] client.

     -  Added `lookupObject()` function.
     -  Added `lookupWebFinger()` function.

 -  `Federation.handle()` now responds with `Access-Control-Allow-Origin: *`
    header for WebFinger requests.

 -  `fetchDocumentLoader()`, the default document loader, now sends `Accept:
    application/activity+json, application/ld+json` header (was `Accept:
    application/ld+json` only).

[NodeInfo]: https://nodeinfo.diaspora.software/
[WebFinger]: https://datatracker.ietf.org/doc/html/rfc7033
[#1]: https://github.com/dahlia/fedify/issues/1


Version 0.1.0
-------------

Initial release.  Released on March 8, 2024.

<!-- cSpell: ignore Dogeon Fabien Wressell -->
