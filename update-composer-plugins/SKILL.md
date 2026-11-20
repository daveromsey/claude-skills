---
name: update-composer-plugins
description: Update all plugins managed by Composer, generate an update report, and list any plugins that need updates that are not managed by Composer.
user-invocable: true
---

# Update Composer-Managed Plugins

Follow this exact cycle: **Update -> Summarize**.

## Step 1: Update Composer-managed plugins
- Update all Composer-managed WordPress plugins to their latest versions.
- Pin each plugin to the latest version.
- When performing the updates, update the lock file as necessary, but only update the `require` section of the `composer.json` file.

## Step 2: Summarize

### For Composer-managed plugins:
- Generate a copyable table in markdown noting the Plugin Name (only the humam-readable name), Old Version, and New version.

### For Non-Composer-managed plugins:
- Generate a copyable table in markdown listing the plugins that need to be updated that were not part of the Composer updates. For these plugins, attempt to provide a link to the site where the plugin can be downloaded manually.