---
layout: default
title: Configuration
parent: Working with Platform.sh
nav_order: 2
---

# Configuration

The Craft CMS template comes with example files for platform.sh configuration that should be suitable for most Craft CMS applications. Complex structures like Craft multisite will require more thought, and the values within the various config files will need to be edited to match the requirements of the build and application.

## Important Files
`.platform.ap.yaml`
This is the main config file for platform, where the details of the build, mounts etc are all defined. This is also where the app's name / unique identifier is set.

`.environment`
This file is run after deploy and allows us to convert / export platform.sh provided environment variables into the variables our config is expecting. For custom variables (ie: those not provided under a different name by platform.sh) set them via the CLI or https://console.platform.sh UI.

`.platform/services.yaml`
This file defines other attached services that need to communicate with the main application container. The Starter template includes a MySQL DB service with 512MB of disk space allocated.

`.platform/routes.yaml`
This file defines the HTTP routing of the app, allowing for the accepted domains, and redirect rules, to be defined in code. The setup included in this template handles a single domain, and automatically adjusts to use the `default` domain from platform.sh.

## Environment Variables

Platform allows you to set environment variables in multiple ways - via the CLI, via the GUI, and via defining them in `.platform.app.yaml`. It is reccomended to define variables using the GUI, as definitions within `.platform.app.yaml` are not visible via the GUI.

To set "top level" enviornment variables (such as those expected by Craft when using `App::env()`) you MUST prefix the variable name with `env:`.

EG: to create the environment variable `CRAFT_SECURITY_KEY` you must create a variable named `env:CRAFT_SECURITY_KEY` within the GUI or command line.