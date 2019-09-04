---
tags: [GitHub]
title: What is my disk quota?
created: '2019-09-04T07:24:50.078Z'
modified: '2019-09-04T07:25:01.534Z'
---

# What is my disk quota?

GitHub doesn't have any set user disk quotas. We try to provide abundant storage for all Git repositories, although there are hard limits for file and repository sizes. Keeping repositories small ensures that our servers are fast and downloads are quick for our users.

**Tip:** If you regularly push large files to GitHub, consider introducing [Git Large File Storage (Git LFS)](https://git-lfs.github.com/) as part of your workflow. Git LFS works well with [the GitHub Flow](https://guides.github.com/introduction/flow/) and can be used with any large file, regardless of its type. For more information, see "[Versioning large files](https://help.github.com/en/articles/versioning-large-files)."

### [File and repository size limitations](#file-and-repository-size-limitations)

We recommend repositories be kept under 1GB each. Repositories have a hard limit of 100GB. If you reach 75GB you'll receive a warning from Git in your terminal when you push. This limit is easy to stay within if large files are kept out of the repository. If your repository exceeds 1GB, you might receive a polite email from GitHub Support requesting that you reduce the size of the repository to bring it back down.

In addition, we place a strict limit of files exceeding 100 MB in size. For more information, see "[Working with large files](https://help.github.com/en/articles/working-with-large-files)."

**Note:** If you add a file to a repository via a browser, the file can be no larger than 25 MB. For more information, see "[Adding a file to a repository](https://help.github.com/en/articles/adding-a-file-to-a-repository)".

### [Backups](#backups)

Git is not adequately designed to serve as a backup tool. However, there are many solutions specifically designed for performing backups that are worth checking out, including [Arq](https://www.arqbackup.com/), [Carbonite](http://www.carbonite.com/), [Mozy](http://mozy.com/) and [CrashPlan](https://www.crashplan.com/en-us/).

### [Database dumps](#database-dumps)

Large SQL files do not play well with version control systems such as Git. If you are looking to provide your developers with the most recent production dataset, we recommend using [Dropbox](https://www.dropbox.com/) for sharing files like these among your developers.

If you are looking to backup your production servers, see the **Backups** section above.

### [External dependencies](#external-dependencies)

Another thing that causes Git repositories to become large and bloated are external dependencies. It's best to leave these files out of the repository and use a package manager instead. Most popular languages come with package managers that can do this for you. [Bundler](http://bundler.io/), [Node's Package Manager](http://npmjs.org/) and [Maven](http://maven.apache.org/). They each support using a Git repository directly as well, so you don't need pre\-packaged sources.

### [Packaged release versions](#packaged-release-versions)

We don't recommend distributing compiled code and pre\-packaged releases within your repository. For more information on sharing large files, see "[Distributing large binaries](https://help.github.com/en/articles/distributing-large-binaries)."

### [Changing history of an existing repository](#changing-history-of-an-existing-repository)

If you already have a repository that's quite large, don't fret! You can remove large files from the repository's history to reduce its size. For more information, see "[Removing files from a repository's history](https://help.github.com/en/articles/removing-files-from-a-repository-s-history)."

### Ask a human


[What is my disk quota? - GitHub Help](https://help.github.com/en/articles/what-is-my-disk-quota)

