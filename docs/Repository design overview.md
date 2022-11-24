## Repositories naming

All CACHET Research Platform (CARP) repositories follow the naming standard described below:

`carp.<subsystem>.<component>-<language>`

Language specific implementation are added with a `-` to the subcomponent. For example:

[`carp.core-kotlin`](https://github.com/cph-cachet/carp.core-kotlin)

which is the implementation of the carp.core domain model in Kotlin.

## Branch naming

In CARP we use four main types of branches:

 * **`master`** - the main branch containing the stable version which is released to "production". e.g. the production or staging server, or an app available in the app store.

 * **`test`** - the latest stable test version which is available on a testing environment, e.g. the test server or a test version on Apple TestFlight or Google Internal Testing.
* **`develop`** - the development branch, where new development is merged.
* `develop-<feature>` - a specific feature being developed as a branch of the `develop` branch.

The merge policy is always to create a **Pull Request** and merge in this order:

 * merge features into `develop`
 * merge `develop` into `test`
 * merge `test` into `master`

This is the overall picture of how we use branches.

```
  master <-- test <-- dev <--+ <-- dev-feature-1
                             | <-- dev-feature-2
                             | <-- dev-feature-3
```

## Using Issue Tracker, Pull Requests, and Releases

Bug and other issue are to be reported in the GitHub issue tracker. Issue are for "backward looking" issues, i.e. stuff that needs to be fixed. For "forward-looking" things that are to be developed (e.g. new features), this is handled by our Trello board(s). 

When merging stuff from dev > test > master, we **always** use Pull Requests with a prober description.

Major releases to production is tagged as a release and have a prober version number and description.





