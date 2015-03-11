---
title: CMS Offline Software
layout: default
related:
 - { name: "Project page", link: "https://github.com/cms-sw/cmssw" }
 - { name: "Feedback", link: "https://github.com/cms-sw/cmssw/issues/new" }
---

# CMS Externals

In order to simplify maintainance and backup of [CMSSW](https://github.com/cms-sw/cmssw)
externals we have set up a separate organization where we keep copies of their sources.

The complete list of externals maintained this way can be found at:

<https://github.com/cms-externals/>

While this does not yet include all of the externals yet (some still use the original tarballs)
the number of externals tracked this way will increase as time goes by. A notable exception
to this is the root repository, which is currently hosted under the main cms-sw organization.

There are actually two ways we keep track of changes:

- Full mirrors
- Partial mirrors

Full mirrors are for those repositories which are already managed in github or git.

Partial mirrors are for those cases in which a different version control system
(VCS) is used (e.g. CVS) or in which we do not want to keep track of the whole 
set of changes. In that case what we do is to keep single commits with the
contents of the pristine tarballs.


## Branch structure

In case of a *full mirror*, we maintain the branch structure as found in the
original mirror. CMS patches are applied on top of the original tags by forking
a new branch `cms/<original-tag-or-commit>`.

For example the official xrootd `v4.0.3` tag is at:

<https://github.com/cms-externals/xrootd/tree/v4.0.3>

and CMS patches on top are at:

<https://github.com/cms-externals/xrootd/tree/cms/v4.0.3>

In case of a *partial mirror*, we commit on the `master` branch the unpacked
pristine tarball, we tag it with the original tag and we keep CMS specific patches
as `cms/<original tag>`.

This allows us to quickly understand what is being used by our externals and 
to do a diff between our version and the original one. For example our
additions on top of xrootd can be seen by doing:

```
git diff v4.0.3..cms/v4.0.3
```

or by using github GUI:

<https://github.com/cms-externals/xrootd/compare/v4.0.3...cms/v4.0.3>
