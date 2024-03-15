---
title: Home
layout: home
nav_order: 1
---

# Renewal SA Craft CMS Template Docs

The Craft CMS acts as a blank canvas to allow agencies to easily spin up new Craft CMS 4 projects that conform with requirements for Renewal SA and the Platform.sh hosting environment that sites are deployed to.

This repository is intended to be cloned, then detached from this git remote. You are free to change from there as you see fit - there is no need to keep this repo as an upstream dependency, the project is not designed to "ship updates".

{:.warning}
Naming matters. The project will need a "slug" or ID - it's best if this matches the repository name, so that if any other developer clones the project it will have the correct folder name to match the committed config. Please be mindful of this when naming the initial repository.

## Getting started

### Requirements

The craft template uses [DDEV](https://ddev.readthedocs.io/) for the local development environment - this is reccomended by both the Craft Project (big fans of DDEV) and Platform.sh (the major sponsor of DDEV).

Ensure you have DDEV installed before following these instructions.

{:.note}
Developers can utilise other local environments, but instructions are not provided. [Valet](https://laravel.com/docs/10.x/valet) and [Herd](https://herd.laravel.com/) are examples of alternative development environments that are known to work.

### Clone and init your repository

To utilise this project, clone the git repo to your local machine, remove the current git repo and initialise a new one. Renewal SA should provide you with a repository within their GitHub organisation to act as the primary `origin` remote.

The name of the folder will affect the default domain that DDEV provisions, in the following example the project slug / folder name is `my-project` which would translate to the site being available at `https://my-project.ddev.test`

```bash

# Clone the Repo

git  clone  https://github.com/renewalsa-websites/craft-template.git  my-project

# Navigate into the project folder

cd  my-project

# Remove the current git repo

rm  -rf  .git/

# Remove the docs folder and github build actions

rm -rf docs/
rm -rf .github/

# Start a new git repo

git  init

# Initial Commit

git  add  .

git  commit  -m  "Initial commit"

# Add git remote in Renewalsa websites organisation (to be provided to you)

# Push initial code to the remote

git  remote  add  origin  https://github.com/renewalsa-websites/my-project.git

git  branch  -M  master

git  push  -u  origin  master

```

### Replace `craft-template` with your project slug

A number of files will need the `craft-template` string replaced with your project's slug. the slug should correspond to the folder name locally for ease of setup.

Replace in the following files
- `.app.platform.yaml` - `name` property
- `.ddev/config.yaml` - `name` property
- `.platform/routes.yaml` - `upstream` property
- `.env` / `.env.example.dev` - `PRIMARY_SITE_URL` 

### Install Craft CMS
To install craft for the first time, use the following.

```bash
# Install our dependencies (including Craft)
ddev composer install

# Run the craft installer
ddev craft install

## Open the site
ddev launch

```

If another developer has already installed the site, or a staging version exists, the process would be slightly different.

```
# Install our dependencies (including Craft)
ddev composer install
```

Next, manually add the `CRAFT_APP_ID` and `CRAFT_SECURITY_KEY` to `.env`, getting the values from the other install or staging enviornment.

To complete the setup, Import a copy of the existing database and sync the assets.

Depending on the complexity of your project, you may need to add more values to the `.env` to match the other environment - this will depend on the customisations made to craft, along with the plugins used etc.

```
## Open the site
ddev launch
```

## A starting point

This boilerplate is intentionally sparse in order to not force a particular toolchain onto developers. It is mainly concerned with the structure of the project, and the use of composer for managing WordPress, Plugins and 3rd party libraries.

### Key principles

This project is based around the following principles

- Track all code for the project in version control with dependencies managed via composer
- Writable files and directories to be separated out for ease of use with platform.sh
- All volumes to be contained within a dedicated volumes folder, with symlinks for public serving of assets
- Usage of enviornment variables to keep passwords, keys and secrets from being committed to the repo

### Building your site

This project does not provide pre-built templates or dictate your front-end toolchain. 

In most modern projects there will be some kind of processing of javascript, css files etc - you are free to implement what you choose. 

Tailwind.css is a good recommendation for styling though you will often need to use the `@apply` rules to target markup provided by plugins.

Please see [Best Practices](#) for more information on recommended development practices.

## Next Steps

### Develop your site

The site should be developed locally by the developers, with changes committed to git and pushed to GitHub

Please see [Best Practices](#) for more information on recommended development practices.

### Deploy to Platform.sh

When it is time to show the client, it's reccomended to deploy to the Platform.sh server for previewing etc.

The default deployment on platform.sh runs off the `live` branch of the connected repository. Push your changes to a branch called `live` to trigger the deployment.

After the site is live, you should create and use the `uat` branch first for testing and to get sign-off on any changes, and then push to `live` when you are satisfied.

Please see [Deploying to Platform.sh] for more detail.