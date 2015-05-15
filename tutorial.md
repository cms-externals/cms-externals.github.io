---
title: CMS Offline Software
layout: default
related:
 - { name: "Project page", link: "https://github.com/cms-sw/cmssw" }
 - { name: "Feedback", link: "https://github.com/cms-sw/cmssw/issues/new" }
---

## Creating a repo for sources which are not maintained in git / github

For project which do not maintain their codebase in git / Github, we simply
import the tarballs for new releases in the master branch of repository named
as the project itself (e.g. `cms-externals/nss` for the `nss` library).

- Create the repo by going to
  <https://github.com/organizations/cms-externals/repositories/new>. Simply
  specify the repository name and add "`<project>` sources as used by CMS,
  please   refer to `<original web site>` for the official sources". Do not select the "Add README" checkbox.

- Download the tarball and unpack it and cd to the sources directory. E.g.:

      curl -L -O https://ftp.mozilla.org/pub/mozilla.org/security/nss/releases/NSS_3_17_4_RTM/src/nss-3.17.4.tar.gz
      tar xzvf nss-3.17.4.tar.gz

- Initialize the local git repository, add sources and commit:

      cd nss-3.17.4
      git init
      git add -A
      git commit -m'Tarball as provided by https://ftp.mozilla.org/pub/mozilla.org/security/nss/releases/NSS_3_17_4_RTM/src/nss-3.17.4.tar.gz'
      git remote add cms-externals git@github.com:cms-externals/nss.git
      git push cms-externals master

  make sure you specify in the commit where you picked up the tarball from.

- Tag and push the tag:

      git tag v3.17.4
      git push cms-externals v3.17.4

  make sure tags have a format as close as possible to vX.Y.Z, to homogenize
  things.

- Create a `cms/<tag>` branch, which you can use to push CMS specific patches:

      git checkout -b cms/v3.17.4 v3.17.4
      git apply nss-3.17.4-sqlite.patch
      git commit -a -m 'Imported from nss-3.17.4-sqlite.patch'
      git apply nss-3.17.4-sqlite.patch
      git commit -a -m 'Imported from nss-3.17.4-zlib'
      git push cms-externals cms/v3.17.4

## Updating your spec file to pick up sources from github

Once you have uploaded your new external in Github you now need to make sure
that your spec file picks them up when building. To do so you need to have the
following lines in your CMSDIST `.spec` file:

    %define tag <commit>
    %define branch cms/<branch point>
    %define github_user cms-externals
    Source: git+https://github.com/%github_user/<your-extenal>.git?obj=%{branch}/%{tag}&export=%{n}-%{realversion}&output=/%{n}-%{realversion}-%{tag}.tgz

where `<commit>` is the actual commit hash which points to the sources you want
to build and `<your external>` is the name you gave to the repository.
