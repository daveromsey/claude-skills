---
name: update-composer-plugins
description: Update all plugins managed by Composer, generate an update report, and list any plugins that need updates that are not managed by Composer.
user-invocable: true
---

# Update Composer-Managed Plugins

Follow this exact cycle: **Discover -> Preflight -> Update -> Summarize**.

## Step 1: Discover the project

- Locate the repository root and the Composer project root (the directory containing `composer.json`). Do not assume the current directory is the project root. The project root is typically the `wp-content` directory. Start by looking for that.
- Confirm that `composer.json` and `composer.lock` are present. If multiple Composer projects are found, identify the relevant project by checking the WordPress installation structure and package list; stop and ask the user to choose if the correct project remains ambiguous.
- Identify the WordPress layout and managed package conventions, such as Bedrock, a standard WordPress installation, or a custom structure.
- Inspect `composer.json` to confirm that WordPress plugins are Composer-managed and to identify plugin package names, constraints, and package repositories.
- Determine whether WordPress core or plugin updates are constrained separately. Limit the workflow to plugin packages unless the user explicitly requests otherwise.
- Discover manually managed plugins separately by comparing WordPress/WP-CLI plugin data with the Composer-managed plugin package list.

## Step 2: Preflight before updating

- Check the Git working tree before making changes:
  - If it is clean, record the starting branch and commit so changes can be reviewed and reverted.
  - If it is dirty, do not overwrite or mix existing work. Warn the user and either stop or proceed only with an explicit backup/stash/recovery plan.
- Do not commit anything.
- Run `composer validate` before making changes and report any validation errors.
- Run `composer outdated --direct` to identify available direct dependency updates and confirm which listed packages are plugins.
- Review the relevant `composer.json` constraints before updating. Confirm that the selected plugin packages permit the intended update level and do not implicitly expand the update scope to WordPress core, themes, or unrelated dependencies.
- Stop if the project layout, Composer root, plugin ownership, or dirty working state cannot be safely resolved.

## Step 3: Update Composer-managed plugins
- Update only the Composer-managed WordPress plugin packages that the user or project policy identifies for updating.
- Use Composer to resolve and update their pinned versions as needed.
- Update `composer.lock` as necessary. Only update plugin-related entries in the `require` section of `composer.json`, unless the user explicitly requests a broader change.
- After the update, run `composer validate` again and verify the lock file is consistent with `composer.json`.

## Step 4: Summarize

### For Composer-managed plugins:
- Generate a copyable table in markdown noting the Plugin Name (only the human-readable name), Old Version, and New Version.

### For Non-Composer-managed plugins:
- Generate a copyable table in markdown listing the plugins that need to be updated that were not part of the Composer updates. For these plugins, attempt to provide a link to the site where the plugin can be downloaded manually.