# [YAS3FS](http://yas3fs.ai-integration.biz) #

YAS3FS started as a project to refurbish [wins3fs on SourceForge](http://wins3fs.sourceforge.net) for a cloud vendors Windows targetting intentions but it was found to be inadequate so that project
was cancelled and the scope changed to be an exemplar of DDD and x-platform dot net development.
 

This &sect;'s header links the project home, followed by binary install overview (Details are at the /doc path at the home domain) and is followed by the original s3fs README. The forked s3fs and binaries for all platforms are here, everything else is in the home domain.

# Getting Started #

  The 1.0 dot net core elements for linux, mac, and windows are common GUI elements presented on all three platforms and operating against
  standard s3fs on the first two and optionally using the YAS3FS driver. Unix s3fs doesn't run on Windows so the YAS3FS driver isn't optional there.
   
  Since unix s3fs is not modified for YAS3FS 1.0, the recommended action for non developer use is to do the regular s3fs installs for those platforms,
  i.e with port or brew for Mac, the distro's repo for linux as well as the dot net core/mono elements.

  In 2.0 and later the YAS3FS driver will be the default on all platforms but in 1.0 it is enabled for testing only.

## Linux ##

  There's no install for linux in the 1.0/2.0 epoch, you have to build from the sources in the home repo.
  
## Mac OSX ##

  The Mac app installs in the normal way from the binary (dmg). Install [OS X Fuse](https://osxfuse.github.io) first, then install the dmg here.

## Windows ##

  YAS3FS operates as a dot net extension of WinFsp so you need to first install [WinFsp](http://www.secfs.net/winfsp/download/).

  After that, assuming you defaulted everything, you can install the MSI here in the normal way.

# Original S3FS Readme #

## s3fs

s3fs allows Linux and macOS to mount an S3 bucket via FUSE.
s3fs preserves the native object format for files, allowing use of other
tools like [AWS CLI](https://github.com/aws/aws-cli).
[![Build Status](https://travis-ci.org/s3fs-fuse/s3fs-fuse.svg?branch=master)](https://travis-ci.org/s3fs-fuse/s3fs-fuse)

### Features

* large subset of POSIX including reading/writing files, directories, symlinks, mode, uid/gid, and extended attributes
* compatible with Amazon S3, Google Cloud Storage, and other S3-based object stores
* large files via multi-part upload
* renames via server-side copy
* optional server-side encryption
* data integrity via MD5 hashes
* in-memory metadata caching
* local disk data caching
* user-specified regions, including Amazon GovCloud
* authenticate via v2 or v4 signatures

## Installation

Many systems provide pre-built packages:

* Amazon Linux via EPEL:

  ```
  sudo amazon-linux-extras install epel
  sudo yum install s3fs-fuse
  ```

* Debian 9 and Ubuntu 16.04 or newer:

  ```
  sudo apt-get install s3fs
  ```

* Fedora 27 or newer:

  ```
  sudo yum install s3fs-fuse
  ```

* RHEL and CentOS 7 or newer through via EPEL:

  ```
  sudo yum install epel-release
  sudo yum install s3fs-fuse
  ```

* SUSE 12 and openSUSE 42.1 or newer:

  ```
  sudo zypper install s3fs
  ```

* macOS via [Homebrew](https://brew.sh/):

  ```
  brew cask install osxfuse
  brew install s3fs
  ```

Otherwise consult the [complation instructions](COMPILATION.md).

### Examples

s3fs supports the standard
[AWS credentials file](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html)
stored in `${HOME}/.aws/credentials`.  Alternatively, s3fs supports a custom passwd file.

The default location for the s3fs password file can be created:

* using a .passwd-s3fs file in the users home directory (i.e. ${HOME}/.passwd-s3fs)
* using the system-wide /etc/passwd-s3fs file

Enter your credentials in a file `${HOME}/.passwd-s3fs` and set
owner-only permissions:

```
echo ACCESS_KEY_ID:SECRET_ACCESS_KEY > ${HOME}/.passwd-s3fs
chmod 600 ${HOME}/.passwd-s3fs
```

Run s3fs with an existing bucket `mybucket` and directory `/path/to/mountpoint`:

```
s3fs mybucket /path/to/mountpoint -o passwd_file=${HOME}/.passwd-s3fs
```

If you encounter any errors, enable debug output:

```
s3fs mybucket /path/to/mountpoint -o passwd_file=${HOME}/.passwd-s3fs -o dbglevel=info -f -o curldbg
```

You can also mount on boot by entering the following line to `/etc/fstab`:

```
s3fs#mybucket /path/to/mountpoint fuse _netdev,allow_other 0 0
```

or

```
mybucket /path/to/mountpoint fuse.s3fs _netdev,allow_other 0 0
```

If you use s3fs with a non-Amazon S3 implementation, specify the URL and path-style requests:

```
s3fs mybucket /path/to/mountpoint -o passwd_file=${HOME}/.passwd-s3fs -o url=https://url.to.s3/ -o use_path_request_style
```

or(fstab)

```
s3fs#mybucket /path/to/mountpoint fuse _netdev,allow_other,use_path_request_style,url=https://url.to.s3/ 0 0
```

To use IBM IAM Authentication, use the `-o ibm_iam_auth` option, and specify the Service Instance ID and API Key in your credentials file:

```
echo SERVICEINSTANCEID:APIKEY > /path/to/passwd
```

The Service Instance ID is only required when using the `-o create_bucket` option.

Note: You may also want to create the global credential file first

```
echo ACCESS_KEY_ID:SECRET_ACCESS_KEY > /etc/passwd-s3fs
chmod 600 /etc/passwd-s3fs
```

Note2: You may also need to make sure `netfs` service is start on boot

### Limitations

Generally S3 cannot offer the same performance or semantics as a local file system.  More specifically:

* random writes or appends to files require rewriting the entire file
* metadata operations such as listing directories have poor performance due to network latency
* [eventual consistency](https://en.wikipedia.org/wiki/Eventual_consistency) can temporarily yield stale data([Amazon S3 Data Consistency Model](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html#ConsistencyModel))
* no atomic renames of files or directories
* no coordination between multiple clients mounting the same bucket
* no hard links
* inotify detects only local modifications, not external ones by other clients or tools

### References

* [goofys](https://github.com/kahing/goofys) - similar to s3fs but has better performance and less POSIX compatibility
* [s3backer](https://github.com/archiecobbs/s3backer) - mount an S3 bucket as a single file
* [S3Proxy](https://github.com/gaul/s3proxy) - combine with s3fs to mount Backblaze B2, EMC Atmos, Microsoft Azure, and OpenStack Swift buckets
* [s3ql](https://github.com/s3ql/s3ql/) - similar to s3fs but uses its own object format
* [YAS3FS](https://github.com/danilop/yas3fs) - similar to s3fs but uses SNS to allow multiple clients to mount a bucket

### Frequently Asked Questions

* [FAQ wiki page](https://github.com/s3fs-fuse/s3fs-fuse/wiki/FAQ)
* [s3fs on Stack Overflow](https://stackoverflow.com/questions/tagged/s3fs)
* [s3fs on Server Fault](https://serverfault.com/questions/tagged/s3fs)

### License

Copyright (C) 2010 Randy Rizun <rrizun@gmail.com>

Licensed under the GNU GPL version 2

