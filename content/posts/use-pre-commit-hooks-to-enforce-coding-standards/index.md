---
title: "Use Pre-Commit Hooks to Enforce Coding Standards"
date: 2024-07-04T12:32:59+01:00
draft: false
summary: "As a quick follow up to my piece on ditching blocking code reviews, here is an example of automating a check that is frequently done manually."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
tags: ["Continuous Integration", "Continuous Delivery", "Git"]
toc:
  enable: false
---

As a quick follow up to [my piece on ditching blocking code reviews](/2024/07/pull-request-theatre/), here is an example of automating a check that is often done manually.

## Git Hooks

Git hooks are script hooks that can be executed before or after a specific set of events. There are both client and server hooks. Client hooks execute on local repositories and server hooks execute on the remote host of a repository. 

When you create a git repository you can find examples of different hook scripts in `./git/hooks`. One thing to keep in mind when you are using them is that hook scripts need to be explicitly copied to the `./git/hooks` directory. They cannot be used directly from a repository.

## A Quick Word On Code Style

Hopefully no-one is surprised to learn that pretty much every programming language has one or more formatting and linting tools that can be used to enforce a coding standard. The vast majority of these are configurable to allow you to choose preferred style and usage rules. 

It is absolutely worth codifying your preferred coding standards in configuration files, as not only can they be used in your CI infrastructure they can be shared with the team and used in most IDEs and editors to provide real time feedback.

## A Simple Example

Here is a very simple example of a pre-commit hook that automatically formats C# and JavaScript source files before they are committed.

```sh
#!/bin/bash

# Use dotnet format to format .cs files
CHANGED_CS_FILES=$(git diff --name-only --cached --diff-filter=ACM | grep '\.cs$')
if [ -n "$CHANGED_CS_FILES" ]; then
    dotnet format --include $CHANGED_CS_FILES
fi

# Use prettier to format javascript files.
git diff --name-only --cached --diff-filter=ACM | grep '\.js$' | while read -r file; do
    npx prettier --write "$file"
done

# Re-add reformatted files to staging to include them in the commit.
git diff --name-only --cached --diff-filter=ACM | grep -E '\.(cs|js)$' | while read -r file; do
    git add "$file"
done

exit 0
```

You can find a simple repository [here](https://github.com/christopher-bimson/AutoFormatDemo/blob/master/README.md) that you can test with by messing up the formatting of the example files then committing them. You can create a script like this to cover a multitude of file types and their matching tools.

{{< admonition type=warning title="Careful!" open=true >}}
The example repository makes the assumption that the users environment already has the required tools to run the hook script. 

**Don't do this in a real repository.**

Developers should be able to pull a new repository and automatically configure (e.g. via a script) their environment so that they can build, test and run the software immediately. 

Don't make assumptions about environment setups and don't rely on documentation and/or the manual setup of development environments.
{{< /admonition >}}