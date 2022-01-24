---
title: Troubleshooting microblog
date: '2022-01-24T09:25:00.000Z'
---

It has been a while since I have posted, and it seems like the Netlify build is failing for this microblog.

```
9:20:54 AM: Build ready to start
9:20:54 AM: ---------------------------------------------------------------------
  UNSUPPORTED BUILD IMAGE

  The build image for this site uses Ubuntu 14.04 Trusty Tahr, which is no longer supported.

  To enable builds for this site, select a later build image at the following link:
  https://app.netlify.com/sites/brave-ardinghelli-70d541/settings/deploys#build-image-selection

  For more details, visit the build image migration guide:
  https://answers.netlify.com/t/end-of-support-for-trusty-build-image-everything-you-need-to-know/39004
```

I followed the blog post and updated my build image to `Ubuntu Focal 20.04 (default)`. Time to retry.

----

Build failed.

```
9:29:49 AM: error type-fest@2.10.0: The engine "node" is incompatible with this module. Expected version ">=12.20". Got "10.24.1"
9:29:49 AM: error Found incompatible module.
9:29:49 AM: info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
9:29:49 AM: Error during Yarn install
9:29:49 AM: Build was terminated: Build script returned non-zero exit code: 1
```

I am not going to try to `Clear cache and deploy site`, in case it was caching a node module from `trusty`.

----

Build failed.

```
9:40:44 AM: > Failed to download https://yarnpkg.com/downloads/1.17.0/yarn-v1.17.0.tar.gz.
9:40:44 AM: mv: cannot stat '/opt/buildhome/.yarn': No such file or directory
9:40:44 AM: No yarn workspaces detected
9:40:44 AM: Started restoring cached node modules
9:40:44 AM: Finished restoring cached node modules
9:40:44 AM: /opt/build-bin/run-build-functions.sh: line 141: yarn: command not found
9:40:44 AM: Installing NPM modules using Yarn version
9:40:44 AM: /opt/build-bin/run-build-functions.sh: line 152: yarn: command not found
9:40:44 AM: Error during Yarn install
9:40:44 AM: Build was terminated: Build script returned non-zero exit code: 1
```

----

- `npm run dev` - works locally on macOS 12.1 -- serving content at `http://localhost:8000/`
- same with other `npm run` commands from package.json

----

The [blog post](https://answers.netlify.com/t/please-read-end-of-support-for-trusty-build-image-everything-you-need-to-know/39004#how-can-i-make-my-site-work-with-the-new-image-6) does a good job of describing the issues that I am running into with the new build image. Specifically I am running into issues with `yarn 1.17`. I am not sure why it is trying to install that when the default should be `yarn 1.22` per the [docs](https://github.com/netlify/build-image/blob/focal/included_software.md#tools). I am going to set it to `yarn 1.22` and rebuild.

----

Build failed.

```
10:05:25 AM: error type-fest@2.10.0: The engine "node" is incompatible with this module. Expected version ">=12.20". Got "10.24.1"
10:05:25 AM: error Found incompatible module.
10:05:25 AM: info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
10:05:25 AM: Error during Yarn install
10:05:25 AM: Build was terminated: Build script returned non-zero exit code: 1
```
