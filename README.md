# Reskill Americans Learning

This is repository for the 2022 Reskill Americans website.

View the current [staging version here](https://reskill-learning.web.app/).

This will combine the "marketing" website with a log-in site for participants.

The primary features here are:

- Collecting user information, including linking to LinkedIn identity
  for all participants.
- Collecting ongoing metrics via surveys, content engagement, and course
  completions so we have longitudinal data on all participants, progess, and
  success rates.
- Comprehensive skills database by which to measure job readiness.
- Learning pathways designed to teach the required skills.
- Mentor/participant matching - grouping participants into small learning
  "pods" to build peer support and community.
- Collect progress information on skills acquisition and project completions.
- Logins for participants, paid and volunteer mentors, and administrators.
- Reporting on participant activities and survey results.

# Technology Choices

In as much as possible, this will be a staticly generated web site using
[Hugo](https://gohugo.io/). As such, the primary templating language will be
[Go Templates](https://pkg.go.dev/text/template).

Some data will be rendered dynamically from source-controlled JSON datasets
(e.g., the Skills database).  We will use React for client-side rendering.

The backend will use [Firebase](https://firebase.google.com/) incuding
[Google Cloud Firestore](https://firebase.google.com/docs/firestore) for
collecting and reporting data, user-authentication, Firebase functions for code
that needs to run in a secure context (such as OAuth validation).

All code is in TypeScript, and uses Mocha/Chai for testing and continuous
integration.

# Using this repository

The tooling for this project has been tested using Linux.  It probably also
works on a Mac - but would need some tweaking to use Windows (perhaps using WSL
or GitBash).

To get started:

```
$ source tools/use
$ configure
$ build
$ run-tests
$ test-server
```

When making content and css changes, it is convenient to automatically process
website updates:

```
$ hugo-watch
```

Only PR's that have passed testing can be merged to the main
branch (and only FF commits are allowed in main).

## Video update process

This repo includes content files that are generated from YouTube video
files from our [YouTube Channel](https://www.youtube.com/c/ReskillAmericans).

Data is stored in two files in the data directory:

- `youtube-videos.json`<br>
  This is all updated via the YouTube data api.  It contains data
  on all our uploaded videos and playlists.  Refresh this information
  by running `get-video-data.mjs`.  *You will need an api-key to
  do this. Run `api-keys-update --decrypt` if you have the password
  for the shared (secret) api key.*
- `video-metadata.toml`<br>
  This file can be updated by running `update-video-metadata.mjs`.
  This will grab any updated information from `youtube-videos.json`
  and create new metadata block for new videos - or update any
  missing metadata for already listed videos (if will NOT EVER
  change any metadata that has been hand-edited here).<br>
  ***But warning: comments in this file will be stripped by running this command.***

The `create-video-content-files.mjs` command will regenerate ALL the video
content files in the `content/videos` directory by combining the information in the `video-metadata.toml` file with the `youtube-videos.json` file.

A sample metadata entry for a video looks like this:

```
[g7hCp8is2qg]
title = "Reskill Americans Town Hall #18 | Andrew Kwatinetz and Eric Patey - Sonos"
videoId = "g7hCp8is2qg"
guest = "Andrew Kwatinetz & Eric Patey"
guestTitle = "Sonos"
slug = "town-hall-18-andrew-kwatinetz-eric-patey-sonos"
num = "18"
filename = "2021-08-02-g7hCp8is2qg.md"
date = "2021-08-02T15:01:23.000Z"
draft = false
tags = [ "town-halls" ]
quote = "This is a sample pull-quote for the video."
```

All but the filename will be added to the [Front Matter](https://gohugo.io/content-management/front-matter/)
of the corresponding Markdown file.

## Directory Structure

| Directory | Usage |
--- | --- |
| .github | GitHub actions workflow definitions. |
| node_modules | Node/npm installed packages. |
| src | TypeScript source files. |
| static | Files to be copied to server "as is" |
| static/scripts | Bundled client-side JavaScript files - compiled from TypeScript |
| data | Hugo data files |
| content | Hugo content files |
| archetypes | Hugo file types |
| themes | Hugo theme files |
| resources | Hugo caches? |
| public | Hugo builds website here.  This directory is copied to Firebase Hosting. |
| tools | Utility commands for this repo. |
| bin | External tools installed here (e.g. Hugo) |
