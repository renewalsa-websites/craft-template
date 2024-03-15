---
title: Configuration and setup
layout: default
nav_order: 3
---

# Configuration

Craft is a flexible system and has multiple ways to handle config - all of them can/will work but it's important to consider the multi-environment nature of Renewal SA sites, along with the security considerations for sensitive configuration or credentials.

The following table can help you understand where best to set these values.

| Environment Specific Value? | Sensitive Value (API key etc)  | Strategy  |
|--|--|--|
| Yes | Yes | Environment Variables |
| No | Yes  | Environment Variables |
| Yes | No | Environment Variables |
| No | No | PHP Config, `.environment` file |

As you can see, Enviornment Variables are often the best answer, but for non-sensitive values that stay the same between the enviornments, it can be best to commit these values to the repo via a `config/*.php` file. `.environment` can also be used to export environment variables at runtime, allowing you to "translate" platform.sh specific vars and dynamically set values.

{:.warning}
Do not commit API keys, security keys, or other sensitive information to the git repository

## Environment Variables

When using environment variables, local variables can be set in `.env`. As more variables are used in the project, it is reccomended to update the `.env.example.dev` with placeholders to show which variables are expected. Do NOT put sensitive values in `.env.example.dev` as this is committed to the repository.

When it comes to deploying on Platform.sh the enviornment variables can be set via the GUI or CLI. It is reccomended to set them [via the GUI](https://docs.platform.sh/administration/web/configure-project.html#variables) on [https://console.platform.sh/](https://console.platform.sh/) as we have noted some inconsistencies with how CLI created variables are shown in the GUI. It is also possible to define variables in `.platform.app.yaml` but these definitions are NOT visible via the GUI.

More information on setting variables on Platform.sh can be found in their docs. 

- [Setting variables via the GUI](https://docs.platform.sh/administration/web/configure-project.html#variables)
- [Variables Overview](https://docs.platform.sh/development/variables.html)

{:.warning}
The most overlooked info in this doc is that variable names must be prefixed with `env:` to be accessed as 'top level' env vars, others will be made available via `PLATFORM_VARIABLES`. For example, you should make a variable named `env:CRAFT_SECURITY_KEY` in order to access it with `App::env('CRAFT_SECURITY_KEY');`.

## Setup and Folder Structure
This project utilises a folder structure largely based around the default craft installation, as this would be familiar to all craft developers.

### Web directory
The web accessible directory is still called `web` as this is the default. Renaming this to `public` is permissable if you require, be sure to update the necessary config in `bootstrap.php` and `.platform.app.yaml`.

## License Directory
Craft requires the license path to be writable, otherwise it throws an exception. The license has been moved to `license/license.key`, and the `license` folder is listed as a mount for platform.sh. When pushing to platform.sh for the first time, upload you license key to the mount via the CLI, or define the license key in an enviornment variable

## Storage Directory
This install assumes the storage directory is ephemeral and will not persist between environments. It is possible to sync / upload this to platform.sh as with any other mount, but this should not be required.

## Volumes
This folder is designed to contain all volumes for storing assets. The public and private folders are to provide easy reasoning regarding the visibility of these assets and should correspond with their values on the Craft CMS filesytem (has public URL etc).

All of these folders will not be web accessible by default - to serve assets from a volume in the `volumes/public` directory, create a symlink from the `web` directory. The symlink found at `web/images` to `volumes/public/images` is an example of this.

The symlink should be relative rather than using absolute paths to work across environments.
```bash
# Navigate into the directory where the symlink will live (web)
cd web

# Create the symlink using relative paths
ln -s ../volumes/public/images images 
```


