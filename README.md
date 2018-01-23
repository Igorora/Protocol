<p align="center">
  <img src="https://static.igorora-project.net/Image/Hoa.svg" alt="Hoa" width="250px" />
</p>

---

<p align="center">
  <a href="https://travis-ci.org/igororaproject/Protocol"><img src="https://img.shields.io/travis/igororaproject/Protocol/master.svg" alt="Build status" /></a>
  <a href="https://coveralls.io/github/igororaproject/Protocol?branch=master"><img src="https://img.shields.io/coveralls/igororaproject/Protocol/master.svg" alt="Code coverage" /></a>
  <a href="https://packagist.org/packages/igorora/protocol"><img src="https://img.shields.io/packagist/dt/igorora/protocol.svg" alt="Packagist" /></a>
  <a href="https://igorora-project.net/LICENSE"><img src="https://img.shields.io/packagist/l/igorora/protocol.svg" alt="License" /></a>
</p>
<p align="center">
  Hoa is a <strong>modular</strong>, <strong>extensible</strong> and
  <strong>structured</strong> set of PHP libraries.<br />
  Moreover, Hoa aims at being a bridge between industrial and research worlds.
</p>

# igorora\Protocol

[![Help on IRC](https://img.shields.io/badge/help-%23igororaproject-ff0066.svg)](https://webchat.freenode.net/?channels=#igororaproject)
[![Help on Gitter](https://img.shields.io/badge/help-gitter-ff0066.svg)](https://gitter.im/igororaproject/central)
[![Documentation](https://img.shields.io/badge/documentation-hack_book-ff0066.svg)](https://central.igorora-project.net/Documentation/Library/Protocol)
[![Board](https://img.shields.io/badge/organisation-board-ff0066.svg)](https://waffle.io/igororaproject/protocol)

This library provides the `igorora://` protocol, which is a way to abstract resource
accesses.

[Learn more](https://central.igorora-project.net/Documentation/Library/Protocol).

## Installation

With [Composer](https://getcomposer.org/), to include this library into
your dependencies, you need to
require [`igorora/protocol`](https://packagist.org/packages/igorora/protocol):

```sh
$ composer require igorora/protocol '~2.0'
```

For more installation procedures, please read [the Source
page](https://igorora-project.net/Source.html).

## Testing

Before running the test suites, the development dependencies must be installed:

```sh
$ composer install
```

Then, to run all the test suites:

```sh
$ vendor/bin/igorora test:run
```

For more information, please read the [contributor
guide](https://igorora-project.net/Literature/Contributor/Guide.html).

## Quick usage

We propose a quick overview of how to list the current tree of the protocol, how
to resolve a `igorora://` path and finally how to add a new node in this tree.

### Explore resources

First of all, to get the instance of the `igorora://` protocol, you should use the
static `getInstance` method on the `igorora\Protocol\Protocol` class which
represents the root of the protocol tree:

```php
echo igorora\Protocol\Protocol::getInstance();

/**
 * Might output:
 *   Application
 *     Public
 *   Data
 *     Etc
 *       Configuration
 *       Locale
 *     Lost+found
 *     Temporary
 *     Variable
 *       Cache
 *       Database
 *       Log
 *       Private
 *       Run
 *       Test
 *   Library
 */
```

We see that there is 3 “sub-roots”:

  1. `Application`, representing resources of the application, like public files
     (in the `Public` node), models, resources… everything related to the
     application,
  2. `Data`, representing data required by the application, like configuration
     files, locales, databases, tests etc.
  3. `Library`, representing all Hoa's libraries.

Thus, `igorora://Library/Protocol/README.md` represents the abstract path to this
real file. No matter where you are on the disk, this path will always be valid
and pointing to this file. This becomes useful in an application where you would
like to access to a configuration file like this
`igorora://Data/Etc/Configuration/Foo.php`: Maybe the `Data` directory does not
exist, maybe the `Etc` or `Configuration` directories do not exist neither, but
each node of the `igorora://` tree resolves to a valid directory which contains your
`Foo.php` configuration file. This is an **abstract path for a resource**.

### Resolving a path

We can either resolve a path by using the global `resolve` function or the
`igorora\Protocol\Protocol::resolve` method:

```php
var_dump(
    resolve('igorora://Library/Protocol/README.md')
);

/**
 * Might output:
 *     string(37) "/usr/local/lib/Hoa/Protocol/README.md"
 */
```

### Register new nodes in the tree

The `igorora://` protocol is a tree. Thus, to add a new “component”/“directory” in
this tree, we must create a node and register it as a child of an existing node.
Thus, in the following example we will create a `Usb` node, pointing to the
`/Volumes` directory, and we will add it as a new sub-root, so an immediate
child of the root:

```php
$protocol   = igorora\Protocol\Protocol::getInstance();
$protocol[] = new igorora\Protocol\Node('Usb', '/Volumes/');
```

Here we are. Now, resolving `igorora://Usb/StickA` might point to `/Volumes/StickA`
(if exists):

```php
var_dump(
    resolve('igorora://Usb/StickA')
);

/**
 * Might output:
 *     string(15) "/Volumes/StickA"
 */
```

## Documentation

The
[hack book of `igorora\Protocol`](https://central.igorora-project.net/Documentation/Library/Protocol)
contains detailed information about how to use this library and how it works.

To generate the documentation locally, execute the following commands:

```sh
$ composer require --dev igorora/devtools
$ vendor/bin/igorora devtools:documentation --open
```

More documentation can be found on the project's website:
[igorora-project.net](https://igorora-project.net/).

## Getting help

There are mainly two ways to get help:

  * On the [`#igororaproject`](https://webchat.freenode.net/?channels=#igororaproject)
    IRC channel,
  * On the forum at [users.igorora-project.net](https://users.igorora-project.net).

## Contribution

Do you want to contribute? Thanks! A detailed [contributor
guide](https://igorora-project.net/Literature/Contributor/Guide.html) explains
everything you need to know.

## License

Hoa is under the New BSD License (BSD-3-Clause). Please, see
[`LICENSE`](https://igorora-project.net/LICENSE) for details.
