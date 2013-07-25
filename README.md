Below are 2 proposals [Proposal A](#proposal-a-code-and-data-reuse) and [Proposal B](#proposal-b-code-reuse).

Then, there is a list of [Features](#features) in both the Unpub (Implemented and TODO) and in the Archive (TODO)
followed by how each Proposal [reduces the list of "Features"](#consolidated-work).

Proposal A (code and data reuse)
=================================

1. store 2 bits for each content UUID:
  - last published ver
  - next published ver (draft)

2. change rhaptos2.repo to use Production DB structure
  (we would need to do this anyway if writing the archive from scratch)

3. start the Archive code with rhaptos2.repo, adding archive bits as needed. Examples:
  - change the published version to be the draft version on publish
  - trigger web-view/exports when last published ver changes
  - store exports


Benefits
---------

Dogfood:

1. Building on existing repo code instead of starting from scratch (less code to maintain)
2. Eating more dogfood

Publish Payload:

3. EPUB publish payload disappears
4. Publish is just a bit switch and trigger

Translate/Validate:

5. no need to translate UUID's on publish
6. no need for separate validation code, only validate on save

Search and Preview:

7. search is exactly the same
8. Web preview is trivial (just send the authenticated user to the Draft version)


Drawbacks
----------

1. Authenticated GET latest is different than unauthenticated GET latest
2. Archive stores at most 1 draft of unpub content
3. Archive stores unpub resources until they are GC'd



Proposal B (code reuse)
========================

Same as Proposal A except:

1. Reuse the same code as rhaptos2.repo
2. Have flags to turn of PUT/POST routes




Features
=========

Checklist of "Features" needed by each repo (`[X]` means "Implemented" and `[ ]` is TODO).

Unpub Repo
-----------

- `[X]` save individual content
- `[X]` edit individual metadata
- `[X]` edit individual folder content
- `[X]` upload resources
- `[X]` add/remove users
- `[X]` serve modules as `{authors:[], body:"<html>"}`
- `[X]` serve collections as `{authors:[], body:"<html>"}`
- `[X]` serve folders
- `[X]` only authenticated users can view content
- `[X]` only authenticated users can edit content
- `[ ]` validate HTML
- `[ ]` preview PDF, EPUB, online
- `[ ]` import from Word (doc -> EPUB service)
- `[ ]` search content I have access to
- `[ ]` search published content
- `[ ]` continue editing content I have published



Archive Repo
-------------

- `[ ]` serve modules as `{authors:[], body:"<html>"}`
- `[ ]` serve collections as `{authors:[], body:"<html>"}`
- `[ ]` publish bulk content (EPUB)
- `[ ]` validate HTML
- `[ ]` translate HTML ID's when publishing
- `[ ]` search published content
- `[ ]` trigger export generation
- `[ ]` save export docs
- `[ ]` **all** users can view content
- `[ ]` only authenticated users can edit (publish) content


Consolidated Work
==================

By combining the codebase we collapse many of the TODO's in **both** Unpub and Archive repos:

- `[X]` serve modules as `{authors:[], body:"<html>"}`
- `[X]` serve collections as `{authors:[], body:"<html>"}`
- `[X]` only authenticated users can edit (publish) content
- `[ ]` validate HTML
- `[ ]` search content
- `[ ]` preview PDF, EPUB, online


By combining the codebase **and** the DB we get rid of several TODO's:

- `[!]` publish bulk content (EPUB)
- `[!]` translate HTML id's when publishing
