mversion - A cross packaging manager module version handler
===

[![NPM version][npm-image]][npm-url] [![Build Status][travis-image]][travis-url] [![Dependency Status][depstat-image]][depstat-url]

Imitates [```npm version```](https://npmjs.org/doc/version.html) to
also work on other packaging files. For those times you have either
have multiple packaging files (like ```bower.json```, ```component.json```,
```manifest.json```) or just not a ```package.json``` file.
```mversion``` can easily bump your version and optionally commit and create a tag.

## `mversion` file support
- `package.json`
- `npm-shrinkwrap.json`
- `component.json`
- `bower.json`
- `manifest.json`
- `*.jquery.json` (jquery plugin files, e.g `plugin.jquery.json`)

## Usage CLI

```
npm install -g mversion
```

```
$ mversion -h

Usage: mversion [ <newversion> | major | minor | patch | build ] [ -m <optional message> ]
(run in package dir).

Update module version in either or all of package.json,
component.json, bower.json, manifest.json.

Run without arguments to get current version.

# Update version
Update version by defining new semver valid version
or a release string (major, minor, patch, build).
--
Ex: "mversion minor"
Ex: "mversion 1.0.1-beta"
--

# Git
Use -m to auto commit and tag. Apply optional message and
use '%s' as placeholder for the updated version. Default
message is 'v%s' where %s is replaced with new version.

--no-prefix (or -n for short) is used when you want to make
a tag without v as prefix. This does not change behaviour of
the message, just the tag name.

--
Ex: "mversion minor -m"
Ex: "mversion minor -m 'Bumped to v%s'"
--
```

### Examples

```
$ mversion patch
New Version: 0.0.6
Updated package.json
Updated component.json
```

```
$ mversion 0.0.5 -m
New Version: 0.0.5
Updated package.json
Updated component.json
Updated manifest.json
Commited to git and created tag v0.0.5
```

```
$ mversion 1.0.0-rc1 -m "Now in wopping v%s"
New Version: 1.0.0-rc1
Updated package.json
Updated component.json
Commited to git and created tag v1.0.0-rc1
```


## Usage API

```
npm install mversion
```

```javascript
var mversion = require('mversion');

mversion.get(function (err, data) {
  /*
    data = {
      'package.json': VERSION,
      'component.json': VERSION
    }
  */
});

mversion.update('minor', function (err, data) { })
mversion.update('major', function (err, data) { })
mversion.update('major', 'Some commit message for version %s', function (err, data) { }) // Will commit/tag
mversion.update('major', 'Message for %s', true, function (err, data) { }) // Will tag without v prefix
mversion.update('patch', function (err, data) { })
mversion.update('build', function (err, data) { })
mversion.update('0.0.1', 'Bumping version', function (err, data) { }) // Will commit/tag
mversion.update('v1.0.1', function (err, data) { })
mversion.update('v1.0.1-beta', function (err, data) { })
mversion.update('v1.0.1-010988', function (err, data) { })
```


### `mversion.get(callback(err, data))`
Get version of all the different package files. See example above.

### `mversion.update(version, [commitMessage, noPrefix, ]callback(err, data))`
Update version of found package files. See examples above.

### `mversion.isPackageFile(filename) : Boolean`
Checks whether or not the given filename is a valid package file type.

Examples:
```javascript
assert.equal(mversion.isPackageFile('package.json'), true);
assert.equal(mversion.isPackageFile('component.json'), true);
assert.equal(mversion.isPackageFile('bower.json'), true);
assert.equal(mversion.isPackageFile('foobar.json'), false);
assert.equal(mversion.isPackageFile('foobar.jquery.json'), true);
assert.equal(mversion.isPackageFile('foojquery.json'), false);
```
## License

[MIT License](http://en.wikipedia.org/wiki/MIT_License)

[npm-url]: https://npmjs.org/package/mversion
[npm-image]: http://img.shields.io/npm/v/mversion.svg?style=flat

[travis-url]: http://travis-ci.org/mikaelbr/mversion
[travis-image]: http://img.shields.io/travis/mikaelbr/mversion.svg?style=flat

[depstat-url]: https://gemnasium.com/mikaelbr/mversion
[depstat-image]: http://img.shields.io/gemnasium/mikaelbr/mversion.svg?style=flat