## production.json ##

A platform-agnostic JSON file (by convention, named `production.json`) that
describes a project's production deployment in terms of the directories and/or
files that are public, private, internal, and/or external.

`production.json` describes the deployment of a project.

Example:
```
{
    "public": [
        "web/"
    ],
    "private": [
        "LICENSE.md",
        "README.md",
        "bin/",
        "src/"
        "vendor/"
    ],
    "internal": [
        "CHANGELOG.md",
        ".git/",
        "src/**/Tests/"
    ],
    "external": [
        "vendor/"
    ]
}
```

## Why? ##

It is standard practice for projects to specify their dependencies (e.g., bundles,
libraries, modules, packages, etc), i.e., what they consume. In contrast, projects
rarely specify how they are deployed. And if they do, there is no convention for
doing so.

| File             | Context | Usage                                                 |
|-==---------------|---------|-------------------------------------------------------|
| `box.json`       | php     | create .phar archives                                 |
| `composer.json`  | php     | install package dependencies; create package archives |
| `Gemfile`        | gem     | install Ruby gem dependencies                         |
| `.gitattributes` | git     | create git archives                                   |
| `package.json`   | npm     | install node module dependencies                      |

It is good practice to deploy only the files required for the application/system
to function. For example, if you deploy from git, you can consult `production.json`
when constructing the deployment tree from your project repository.  The benefits of
not deploying internal files to a public server include:
* mitigation against fingerprinting (e.g., changelog files)
* reducing the attack surface (e.g., test scripts)

## Configuration ##

Projects can choose what properties to set.
This section provides guidance on how the
A deployment tool may choose to `chmod` and/or `chown` these files.

### public ###

A list of paths that are publicly visible when deployed.

If this property is not specified, all project files are assumed to be deployed
and publicly visible.

Possible Applications:
* a deployment tool may further choose to `chmod` and/or `chown` these files
* an audit tool may confirm the permissions, ownership, and/or presence of these files

### private ###

A list of paths are not publicly visible when deployed.

If this property is not specified, no files are preseumed to be private.

Possible Applications:
* a deployment tool may further choose to `chmod` and/or `chown` these files
* an audit tool may confirm the permissions, ownership, and/or presence of these files

### internal ###

A list of paths that should not be deployed.

If this property is not specified, no project files should be held back from deployment.

Possible Applications:
* an audit tool may confirm absence of these files

### external ###

A list of paths corresponding to external (e.g., third-party) dependencies that
also provide `production.json`. Use this property to reduce the maintenance effort
of declaring those paths in other properties.

Possible Applications:
* a deployment tool may recurse through these paths to locate other `production.json` files

## Validation ##

To validate your `production.json`, refer to `schema.json`.

JSON schema validators include:
* https://github.com/justinrainbow/json-schema
* https://www.jsonschemavalidator.net/
* https://json-schema-validator.herokuapp.com/
* https://jsonschemalint.com/#!/version/draft-07/markup/json
