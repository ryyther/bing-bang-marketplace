# Bing Bang Cowork Marketplace

This repo is the Bing Bang team's plugin marketplace for Claude Cowork.
Subscribe once and you'll get every plugin here, plus updates automatically.

## Add this marketplace in Cowork
In a Cowork chat, run:

    /plugin marketplace add itsbingbang/bing-bang-marketplace

(Replace `itsbingbang/bing-bang-marketplace` with wherever this repo actually lives.)

## Install a plugin from it

    /plugin install bing-bang-project-spinup@bing-bang

## What's inside
- **bing-bang-project-spinup** — turns an accepted Bonsai proposal into a fully
  set-up project (tasks with sold hours distributed, Drive folder, pre-filled
  creative brief). Never contacts the client; asks before assigning people.

## Updating a plugin
1. Bump the `version` in the plugin's `.claude-plugin/plugin.json`.
2. Bump the matching `version` in `.claude-plugin/marketplace.json`.
3. Commit and push. The team runs `/plugin marketplace update bing-bang` and gets the new version.
