Title: Depac - The Universaler OS
Date: 21/10/26

# What if Debian was struck by a curse?

That curse is my idea, and TL;DR:

> A Linux distribution built around a minimal, immutable base system and a `golang` package manager daemon, the system is built around this package manager. The base is built from `debian-testing` so it ships recent base libraries. Applications come from pacstall repositories, live in their own per-package directories, and run isolated from one another while sharing a main set of libraries from the base and optionally another provided by the package manager where fit.

## Le name
> Depac? Really?

All I have to say is this:

```
 Debian roots, but dpkg is out,
 That's what the "pac" is all about.
 Go on, mock it, I don't care,
 I'll rename it when goats take the air.
 ```


## Base image

The base is built from `debian-testing` and contains only the minimum needed to boot and run the package manager. It is immutable: nothing modifies it at runtime, and it changes only by replacing the whole image* (more in next section.)

Core libraries such as `libc` live here and form the single main library set that all applications use.

## Release pipeline

CI builds a new image every week. Each build is compared against the previous image, and only the differences are shipped to users over the air. This keeps updates small and the base current.

See why I put an asterisk in the previous section?

## Package source

Pacstall repositories are the source of applications. They sit on top of the base and keep it small.

Today, CVEs are discovered faster than dinner negotiations end, so we **need** bleeding edge packages delivered fast for all.

## Package manager

A new package manager written in `golang`, using a client-server architecture. A *resident* daemon does the work, and clients (CLI, GUI, *evil* spells) talk to it over a Unix socket.

The protocol is versioned and uses request/response calls for quick operations (search, planning, repository management). Long operations such as builds and installs use a job model: the daemon returns a `jobId` immediately, and the client reads a stream of progress, log, and completion events for that job.

And because the *resident daemon* does not depend on who is calling it, any frontend can be built on top.

## Filesystem layout

Packages are organized in a package-centric hierarchy instead of the traditional split by file type (`/usr/bin`, `/usr/lib`, and so on). Each application has its own directory holding its files and its writable state together. ~~Bill Gates rn: >:(~~

> `/system` active OS slot, contains base libs, etc...

> `/system-b` inactive OS slot and target for next update.

> `libs/` shared non-essential libraries that can be updated through the package manager.

> `programs/` where applications and their entire configurations live.

When an application runs, its directories are mounted at the paths that standard .deb-style software expects, so existing packages work without being modified. Removing an application removes its directory which contains all its stuff.

## Isolation

Each package is isolated from the others, and cross-application access happens only through explicitly declared connections (definitely an original concept.) Applications use a shared set of libraries instead of each carrying its own copies, which keeps duplication low and makes removal clean.

## Goals

- Clean installs and removals.
- Per-application isolation.
- Recent packages without ever modifying the base system.
- Single package management daemon that any frontend can use.