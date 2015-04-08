# Identifiers for Repository Objects



>
> There are only two hard things in Computer Science: cache invalidation and naming things.
>  -- Phil Karlton

>
> ... persistence is purely a matter of service, and is neither inherent in an object nor
> conferred on it by a particular naming syntax.
> -- John A. Kunze

## Background and Motivation

The following articles are great background reading on where we're at:

* [Cool URIs don't change](http://www.w3.org/Provider/Style/URI.html)
* [The web of names, hashes and UUIDs](https://joearms.github.io/2015/03/12/The_web_of_names.html)
* [Towards Electronic Persistence Using ARK Identifiers](https://wiki.ucop.edu/download/attachments/16744455/arkcdl.pdf?version=1&modificationDate=1261036800000)

We are currently using the Handle System for our DSpace-based SHAREOK
repository sytem, but find ourselves largely in agreement with the
criticisms presented in John Kunze's paper introducing ARK identifiers
and have no plans to use them in new systems.

In summary: URIs are already pretty good identifiers, good tools
already exists for mapping old URIs to new URIS when required, a
similar level of effort is requird to maintain the mapping between
objects and identifiers over time regardless of what tools are used,
and it's unclear (and maybe even unlikely) that the survival
characteristics of niche tools like Handle are actually better than
standard web infrastructure over the long term.

Our current plan is to use UUID-based URIs as our primary identifier for
repository objects. This was originally suggested to us by Thorny
Staples, and gives us a nice scheme for URIS that:

* have no semantic content based on the internal properties of a
  resource
* have no semantic content based on the ownership, status, or other
  external properties of a resource
* expose minimal technical details of how the request for a URI might
  be fulfilled
* do not require a central registry to mint new identifiers

This means it's likely that are urls will look like:

```
https://example.com/
```

Although we might want to take additionalstructureal cues from ARK in
our final design.

Because UUID-based URLs are not particularly human friendly, we plan
to maintain an additional set of semantic/mnemonic URIs for some
documents as required.


## What not just use ARKs?

ARK is a good standard, and the URI metadata and commitment statements
built in to ARK seem like really good ideas. It's unfortunate that we
won't get those.

However, support for ARK isn't build in to Islandora and it hasn't
been widely adopted by the Islandora or Fedroa communities. There is
an existing [islandora_ark](https://github.com/ksclarke/islandora_ark)
module, but it's a work in progress that hasn't been updated for two
years. Also, the module seems to depend on CDL's EZID service to
generate ARKs.


## UUIDs as Fedora Identifiers

[Fedora Identifiers](https://wiki.duraspace.org/display/FEDORA38/Fedora+Identifiers)
consist of a namespace ID and an object ID. An object ID can be up to
64 characters long and can include letters and numbers as well as any
of "-", ".", "~", "_". Other special characters can be included as
escaped octets (eg %24 for $)

A [UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier)
is a 128-bit number that is typically represented as 36 character
string composed of hexidecimal digits seperated by into groups by
hyphens. This means that Fedora should have no problem making use of
pre-assigned UUIDs as object IDs.


## The state of URI mapping in Drupal

Drupal has good support (via path_auto and related modules) for almost
any kind of URI mapping that we might want to do, but the Islandora
integration with this is still pretty basic.

* [pathauto](https://www.drupal.org/project/pathauto) is Drupal's
  basic tool for associating urls with content.
* [subpathauto](https://www.drupal.org/project/subpathauto) extends
  pathauto so that it works below the node/entity level and an be used
  for things like contact forms (`/user/1/contact`) and vies arguments
  (`node/$/view`)
* [globalredirect](https://www.drupal.org/project/globalredirect) or
  [redirect](https://www.drupal.org/project/redirect) can be used to
  redirect from Drupal's default paths (`node/1`) to paths defind with
  pathauto to ensure a single canonical url is used.
* [islandora_pathauto](https://github.com/Islandora/islandora_pathauto)
  extends pathauto to work with a limited set of Islandora object
  properties, including the fedora PID and label.


### Drupal's UUIDs module

The [Drupal UUID module](https://www.drupal.org/project/uuid) can be
used to add a UUID to Drupal objects. The main purpose of this seems
to be in support of moving content between multiple Drupal sites, and
it probably isn't a good fit at this point. It might eventually come
in to play as part of our Drupal dev process, or with Islandora 2.













