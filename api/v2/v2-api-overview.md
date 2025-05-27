# Lunch Money API Public Spec, Docs, Changelog and Tests

This is a repository of API specifications for [Lunch Money](https://lunchmoney.ap), a delightfully simple personal finance and budgeting tool.   Unlike many personal finance tools, Lunch Money includes a developer API.

This repo includes an OpenAPI specification for the upcoming v2 of the Lunch Money API.  It also includes files used to generate docs and changelogs, and to run tests against a server that implements the spec.

It is designed to be used as a submodule for projects that build implementations of, or clients for, the Lunch Money API.   

# Versioning

The Lunch Money API spec uses a modified version of SEMVER for its versioning methodology as follows
- The major version is the API version.  This will always be 2 in this repo
- The minor version represents the number of main endpoints (or OpenAPI tags) the current version of the spec supports.  For example, a version of the API that supports the /me, /categories, and /transactions endpoints would have a minor version of 3
- The revision number represents the number of updates since the last endpoint was added.  For example, each time changes are made to one of the existing three APIs as described above, the revision number will be bumped.

Details of each version can be found in [./version-history.md](./version-history.md)

## Branch Naming Convention
Each time a new version of the API spec becomes available, it should be pushed to a new branch with the same name as the version number.  For example the first version to use this naming convention has implemented 3 endpoints and was called v2.3.1.

Typically the main branch is a clone of the most recently updated version branch.

## Using this in a repo that implements the API Service

Developers who are implementing the Lunch Money API can add this module as a github subrepo.

### Add this as a github submodule

From the top level directory of the project ensure that a directory called `specs` exists.   Then add this module as a github subrepo by running the following commands

```bash
mkdir specs
git submodule add https://github.com/jpjpjp/lunchmoney-api-spec.git specs/v2
```

This will add the main branch of this repo to the specs/v2 directory in the target project.  Optionally you may choose to add a particular branch to your repo by doing the following:

```bash
git submodule add -b branch_name https://github.com/jpjpjp/lunchmoney-api-spec.git specs/v2
```

Running either of these will create a new file called .gitmodules which will look something like this:

```bash
 [submodule "specs/v2"]
     path = specs/v2
     url = https://github.com/jpjpjp/lunchmoney-api-spec.git
     branch = branch_name
```

Note that the branch portion is only include if -b option was added on the original submodule add command.

Once this is done you can commit the .gitmodule and the new submodule directory file to your repo.

```bash
git add .gitmodules specs/v2
git commit -m "Add specs and tests as a submodule"
```

### Updating the URLs of where the server is running
The OpenAPI spec includes a `servers` section that specifies the URL of a server that implements the API.  This is used in the dynamic documentation which provides a "Try It" functionality for the APIs.

It is also set in the [index.html](./index.html) file that is used to generate to Stoplight powered documentation.

On github this URL is set to where the hosted mock API server is running. During itereative development you may wish to change this to a local version. 

To do this simply run the following:
```bash
cd specs/v2
.husky/pre-commit --revert
```
By default this will set the URL to "http://localhohost:3000".   You can override this by changing the LOCAL_URL environment variable on the command line or by editing [.env](./.env).  

The PRODUCTION_URL environment variable should not be modified without coordinating a change to where the public server will be hosted.

### Updating an existing project to use the specs and tests from the new submodule directory

If you have an existing project that formerly used a version of the specs and tests in a regular directories you will need to make some updates to your project to reference the specs and tests in this submodule as follows:

1) If your project served the API spec and changelog from a local project directory, for example `./public/`, you need to search and replace all the references to that directory in the code that referenced these files and update the path so that it is now `./specs/v2/`.

2) If your existing project that has scripts in top level package.json to run the portman tests, for example from the `portman/` directory, do a search and replace and change this to `./specs/v2/tests`.

3) Finally, if you have an existing `.env-portman` file in the portman directory, copy it to the top directory of your repo and (if necessary) update .gitignore file so that this version will not be committed with your changes. 

4) Test that your updates work by starting your local API service and run the tests using the following command:
    - `npm run run-tests`  (or whatever your script is to generate and run the portman tests)

5) If these tests all pass, delete the legacy directories (ie: `public` and `portman`) from your repo


## Using this in a repo that implements a Lunch Money API Client

Be the first to contribute an update here!

## Tracking changes in the submodule

After you add this repo to your project as a submodule, your local project will have a copy of the files that were in the branch you checked out, at the time you ran the `git submodule` command.  However, the submodule is in what git calls a ["detached HEAD" state](https://git-scm.com/book/en/v2/Git-Tools-Submodules) which means there is no local working branch for tracking changes.   I tend to think of this as simply a "snapshot" of the remote repo.

If you want to pick up new changes from the remote, or eventually check in changes that you make you must first checkout (or create) a branch in your local version.  To do this cd to to the submodule directory and checkout a branch, ie:

```bash
cd specs/v2
git checkout main
git pull origin main
```

Or, if you specified a particular `branch_name` when you initially added the submodule, checkout and pull that branch instead.   Once this is done you can update your branch to pull down any changes from the remote, or push changes you've made locally to the remote.


### Pulling remote updates from this repo into your project
Once you have followed the steps above to connect your submodule to a branch, you can run the following amy time you wish to sync your local copy with any remote changes.

```bash
git submodule update --merge
```

If you are working from a particular branch in the project (say v2.3.1) and want to update your local to a different branch (say v2.3.2), edit your .gitmodule file and add the new branch name, and follow the steps above to associate the submodule to the new branch before running the update command above.

## Updating this repo from your project

If you find yourself needing to update the files in this project, you can commit and push changes directly from your repo.    Before doing so make sure to create a new branch in your submodule that will allow you to make a PR:

```bash
cd specs/v2
git checkout -b v2.3.3
npm i
```

This will create a new local branch called v2.3.3.   It will also install the dependencies, which is really just the husky library that will run a pre-commit script that will ensure that any updates committed will have the production URL in the API Spec and index.html.

Once this is setup you can modify the API spec with the changes that you want and, when ready, you can commit your changes and push them back to the remote repo for the submodule as follows:

```bash
cd specs/v2
git add .
git commit -m "My awesome changes"
git push origin v2.3.3
```

Note that a pre-commit hook will run and revert the URLs in the API Spec and index.html back to the known production URL.

Once this is complete you can navigate to [this repo on github](https://github.com/jpjpjp/lunchmoney-api-spec.git) and open a PR.  Don't forget to update version-history.md and update the tests before submitting one.
