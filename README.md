## svn2git
Subversion to Git migration tool.

Uses git-svn to clone a Subversion repository including all its tags to Git.
Optionally pushes everything to a remote repository.

**Table of Contents**

- [Getting Started](#getting-started)
  - [Install Dependencies](#install-dependencies)
  - [Get help](#get-help)
  - [Get subversion authors mapping](#get-subversion-authors-mapping)
  - [Migrate the repository](#migrate-the-repository)

### Getting Started

**Important**: since version 2.1.0 ```svn2git migrate``` does not asume
[standard layout for subversion repositories](http://blogs.collab.net/subversion/subversion_repo) anymore.
To migrate a subversion repository with standard layout you can use the same options as available on ```git svn clone```:
```bash
$ bin/svn2git migrate [-A|--authors-file="..."] [--remote="..."] [-s|--stdlayout] [-T|--trunk="..."] [-b|--branches="..."] [-t|--tags="..."] [--preserve-empty-dirs] [--placeholder-filename="..."] source
```
Check the migrate command help or the git svn man pages for more information.

#### Install Dependencies
```
git
git-svn
composer install
```

#### Get help
```bash
$ bin/svn2git
```

```
svn2git - the Subversion to Git migration tool. version 1.0.1

Usage:
  [options] command [arguments]

Options:
  --help           -h Display this help message.
  --quiet          -q Do not output any message.
  --verbose        -v|vv|vvv Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug.
  --version        -V Display this application version.
  --ansi              Force ANSI output.
  --no-ansi           Disable ANSI output.
  --no-interaction -n Do not ask any interactive question.

Available commands:
  fetch-svn-authors   Command line tool to fetch author names from an SVN repository.
  help                Displays help for a command
  list                Lists commands
  migrate             Command line tool to migrate a Subversion repository to Git.
  update              Command line tool to update an existing git-svn bridge repository.
```

### Get subversion authors mapping

**Usage**
```bash
$ bin/svn2git fetch-svn-authors [--output="..."] source
```
**Arguments**
```
  source                Subversion repository to fetch author names from.
```

**Options**
```
  --output              Output file. (default: "./authors.txt")
  --help (-h)           Display this help message.
  --quiet (-q)          Do not output any message.
  --verbose (-v|vv|vvv) Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug.
  --version (-V)        Display this application version.
  --ansi                Force ANSI output.
  --no-ansi             Disable ANSI output.
  --no-interaction (-n) Do not ask any interactive question.
```

**Example**
```bash
$ bin/svn2git fetch-svn-authors svn://example.com/svnrepo --output authors-transform.txt
```

Edit the output file if you want to make adjustments to the layout. Default layout is:
```
username = username <username>
```
Capability to inject the layout might be added in the future.


### Migrate the repository

**Usage**
```bash
$ bin/svn2git migrate [-A|--authors-file="..."] [--remote="..."] source
```

**Arguments**
```
  source                Subversion repository to migrate.
```

**Options**
```
  --authors-file (-A)     Path to Subversion authors mapping.
  --remote                URL of Git remote repository to push to.
  --stdlayout (-s)        The option --stdlayout is a shorthand way of setting trunk,tags,branches as the relative paths, which is the Subversion default.
                          If any of the other options are given as well, they take precedence.
  --trunk (-T)           Relative repository path or full url pointing to the trunk of the repository.
                          Takes precedence over the --stdlayout option.
  --branches (-b)        Relative repository path or full url pointing to the branches of the repository.
                          Takes precedence over the --stdlayout option.
  --tags (-t)            Relative repository path or full url pointing to the tags of the repository.
                          Takes precedence over the --stdlayout option.
  --preserve-empty-dirs   Create a placeholder file in the local Git repository for each empty directory fetched from Subversion.
  --placeholder-filename  Set the name of placeholder files created by --preserve-empty-dirs. (default: ".gitkeep")
  --help (-h)             Display this help message.
  --quiet (-q)            Do not output any message.
  --verbose (-v|vv|vvv)   Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug.
  --version (-V)          Display this application version.
  --ansi                  Force ANSI output.
  --no-ansi               Disable ANSI output.
  --no-interaction (-n)   Do not ask any interactive question.
```

**Example**
```bash
$ bin/svn2git migrate svn://example.com/svnrepo -A authors-transform.txt --remote=git@github.com:user/remoterepo.git --stdlayout
```

To update the master or any added branch / tag just execute the migrate command again.
This might show some warnings and errors because of already existing branches and tags. You can ignore those.

A dedicated update command might be added in the future.


## Update an existing repository

**Usage**
```bash
$ bin/svn2git update [--branches="..."] gitsvn
```

**Arguments**
```
  gitsvn                Git-svn repository to be updated.
```

**Options**
```
  --branches            Branches to be updated. (multiple values allowed)
  --help (-h)           Display this help message.
  --quiet (-q)          Do not output any message.
  --verbose (-v|vv|vvv) Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug.
  --version (-V)        Display this application version.
  --ansi                Force ANSI output.
  --no-ansi             Disable ANSI output.
  --no-interaction (-n) Do not ask any interactive question.
```

**Example**
```bash
$ bin/svn2git update --branches=master,latest-testing,latest-production /path/to/git-svn/repository
```
