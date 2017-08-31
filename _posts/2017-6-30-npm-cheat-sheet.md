---
layout: post
sitemap: true
noindex: false
title: Node npm cheat sheet
categories: node npm cheatsheet
---

NPM is a package manager for Node.js packages, or modules if you like. **npm** makes it easy for JavaScript developers to share and reuse code, and it makes it easy to update the code that you're sharing. Here we have a basic _cheatsheet_ of how to use it... enjoy!

 - [npm init defaults](#npm-init-defaults)
 - [Update the global npm version](update-the-global-npm-version)
 - [Installing Packages](#installing-packages)
 - [Version of packages](#version-of-packages)
 - [List installed packages](#list-installed-packages)
 - [Uninstall packages](#uninstall-packages)
 - [Updating packages](#updating-packages)
 - [Search](#search)
 - [Pruning](#pruning)
 - [Simple Scripts](#simple-scripts)


# npm init defaults
The preferences are stored on user home directory: ~/.npmrc
 
**List all npm configuration flags**
```
npm config ls -l
```

**_npm init_ : set default values**
 ```
 npm set init-authour-name "Carlos Decloedt Jr"
 npm set init-authour-email "carlos@decloedtjr.com.br"
 ```
 - other options
   - init-author-url
   - init-license
   - init-version
 
 
**check default values**
 ```
npm set init-authour-name
```
 
**delete default values**
```
npm config delete init-author-url
```

# Update the global npm version
```
npm update npm -g
```

# Installing Packages

**Install packages from npm repo**
```
npm install loadsh --save
npm i loadsh -s
```
 
**Saving DEV Dependencies**
```
npm install loadsh --save-dev
npm i loadsh -D
```
 
**Install packages globally**
```
npm --install gulp -g 
```

**Install package from git repository**
```
npm install git://github.com/npm/npm.git
npm install git+ssh://git@github.com:npm/npm.git#v1.0.27
npm install git+ssh://git@github.com:npm/npm#semver:^5.0
npm install git+https://isaacs@github.com/npm/npm.git
npm install git://github.com/npm/npm.git#v1.0.27
GIT_SSH_COMMAND='ssh -i ~/.ssh/custom_ident' npm install git+ssh://git@github.com:npm/npm.git
```

**Install specific version of an package**

```
npm i underscore@1.8.2 --save --save-exact
npm i underscore@1.8.x
npm i underscore@1.8 --> latest of 1.8.x
```

```
npm i express@<4
npm i express@"3"
```

# Version of packages
A "version" is described by the v2.0.0 specification found at http://semver.org/.

See [semver](https://docs.npmjs.com/misc/semver) for more details about specifying version ranges.
 - **version** Must match version exactly
 - **>version** Must be greater than version
 - **>=version** etc
 - **<version**
 - **<=version**
 - **~version** "Approximately equivalent to version" See semver
 - **^version** "Compatible with version" See semver
 - **1.2.x** 1.2.0, 1.2.1, etc., but not 1.3.0
 - **http://...** See 'URLs as Dependencies' below
 - __*__ Matches any version
 - **""** (just an empty string) Same as *
 - **version1 - version2** Same as >=version1 <=version2.
 - **range1 || range2** Passes if either range1 or range2 are  - satisfied.
 - **git...** See 'Git URLs as Dependencies' below
 - **user/repo** See 'GitHub URLs' below
 - **tag** A specific version tagged and published as tag See  - npm-dist-tag
 - **path/path/path** See Local Paths below


# List installed packages

**List installed packages**
```
npm list
```
 
**List with the dependency level**
```
npm list --depth 1
```

**List packages installed globally**
```
npm list --global true --depth 0
```

**Show more info about installed packages**
```
npm list --depth 0 --long true
npm list --depth 0 --json true
``` 

**List outdated libraries compared to currently installe node_modules**
```
npm outdated
```
 
# Uninstall packages

**uninstall without remove from _package.json_**

```
npm uninstall underscore
```

**uninstall removing from _package.json_**
```
npm uninstall underscore --save
npm un underscore --save
npm rm underscore --save
npm r underscore --save
npm un underscore --save
```

**uninstall from global repository**

just add the _-g_ flag

```
npm uninstall underscore -g
```

# Updating packages

```
npm update
npm update underscore
```

**Update the global npm version**
```
npm update npm -g
```

**Update all global packages**

_With administrator privilidges..._

```
npm i npm@latest -g
npm update -g
```

# Search
```
npm search underscore
```

# Pruning
This command removes "extraneous" packages. If a package name is provided, then only packages matching one of the supplied names are removed.
Extraneous packages are packages that are not listed on the parent package's dependencies list.

Compare packages installed with the _packages.json_, and remove those who are not listed on _packages.json_
```
npm prune
```

Removing devDependencies
```
npm prune --production
```

# Simple Scripts

based on setup in _package.json_ in:
```
"scripts" : {"test" : "node testfile.js"}
```

```
npm test
```



