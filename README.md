# [WINS3FS](https://wins3fs.ai-integration.biz/doc) #

WINS3FS was originally intended to be derived from [wins3fs on SourceForge](http://wins3fs.sourceforge.net) but that project was found inadequate, so the sources were salvaged, with these changes:

   * Drop the base directory elements from wins3fs.sourceforge.net and replace
     with something that works oriented to WinFsp on Windows and s3fs elsewhere.
   * Refactor the Affirma ThreeSharp to use Amazon SDKs
   * Make the new thing platform independent to the extent of having:

       * a dot net 4 extension of WinFsp
       * a dot net core extension of OSX Fuse
       * a dot net core GUI for s3fs


Two levels of WINS3FS are distinguished and were developed at the same time, 1.0 targetting just the extension DLL to be used with WinFsp and
2.0 which has the larger scope above, WINS3FS proper. 1.0 is a dead end branch of the same line of development.

While 2.0 and later are not limited to Windows, and may be generalized beyond S3 (as regular s3fs is), the name is retained for simplicity.

Sections of this file

2. Git repository layout
3. Instructions for installing the current binaries
4. The original readmes for wins3fs and s3fs-fuse.

This &sect;'s header links the documentation and the link at the top of the document links support.

# Repository Layout #

Starred  items  present in my github repo, others in my private git repo.

|File/Folder | Content |
-------------|---------
|   README.md*         | this file |
|   dotNetV1          | original 2008 sourceforge code set|
|   WINS3FS*          | current Mac and Windows binaries |
|   s3fs*              | forked s3fs repo (root at github)|
|   WINS3FS/code     | the VS code project (2.0 and later only)|
|   WINS3FS/VS2K17   | the VisualStudio 2017 solution(s)|
|   WINS3FS/VS2K19   | the VisualStudio 2019 solution (mac, 2.0 and later only)|


 The VS code project is for linux and mono, the VS2K19 is for Mac and OSX Fuse s3fs.

# Binary Installation #

  The 1.0 dot net core elements for linux, mac, and windows are common GUI elements presented on all three platforms and operating against
  standard s3fs on the first two and optionally using the WINS3FS driver for those platforms.

  Unix s3fs doesn't run on Windows so the WINS3FS driver isn't optional there.

  In 2.0 and later the WINS3FS driver will be the default on all platforms but in 1.0 it is enabled for testing only.
   
  Since unix s3fs is not modified for WINS3FS, the recommended action for non developer use is to do the regular s3fs installs for those platforms,
  i.e with port or brew for Mac, the distro's repo for linux.

## Linux ##

  There's no install for linux in the 1.0/2.0 epoch, you have to build from source.
  
## Mac OSX ##

  The Mac app installs in the normal way from the binary (dmg).

## Windows ##

  WINS3FS operates as a dot net extension of WinFsp so you need to first install [WinFsp](http://www.secfs.net/winfsp/download/).

  After that, assuming you defaulted everything, you can install the MSI the normal way.

## 1.0 Common ##

  After installation, operation will be evident in messages presented in the GUI.

# Original Readmes #

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

# wins3fs #

```
WinS3FS by joelhewitt

An Amazon S3 WINFUSE filesystem for Windows

To use:


1.  Firstly create a S3 account at amazonaws.com.
In the course of registering, copy your awsAccessKeyId and awsSecretAccessKey.


2.  Run wins3fs.exe, this will make the WinS3FS.exe.conf file
populating it with your buckets.

If this is the first time you ran winfuse then you will be asked for your
awsAccessKeyId and awsSecretAccessKey keys.  Additionally if you haven't used
Amazon S3 before, you will be asked to create a bucket. Otherwise WinS3FS will
use your existing buckets.

If the help program wins3fs-config.exe is run after winfuse.exe has alread been started, 
wins3fs.exe will need a restart. 

It would not be advisable to run winfuse-config.exe if you are in the middle 
of copying a file from S3 via WinS3FS.

4.  Assuming you were able to run winfuse and enter your aws keys, and
perhaps enter a bucket name, you should be presented with "WinS3FS" icon in
the tray bar. Right clicking on this icon will preset a menu of options.
"Exit" is self explanitory, "Add Bucket" allow you to add extra buckets.
Clicking "show" and/or "hide" will reveal a window with a text output area.
During the alpha/beta release of this software this will show debugging 
information. If WinS3FS is compiled as a service this window will not show.
Note in the future running WinS3FS as a service will no be supported.

5.  open a command prompt and run "net view \\s3" or open a windows explorer
window and type in \\s3, and you should see a list of buckets you have accessable.

6.  if in a command prompt type dir \\s3\<bucket name>  to see the files in that bucket
or in windows explorer open the bcuket you would like to use.

7.  Copy your files.

8.  All files copied to your S3 bucket are made public readable. So if you
had copied a file name "index.htm" into S3, you can now from your web browser
enter "<bucketname>.s3.amazonaws.com/index.htm" and view the file you made.



If the above instructions do not get WinS3FS working, make sure you have .Net
intalled on your computer.  Make sure you did not delete a quotation mark in
the WinFUSE.exe.conf-template.  Do you have a bucket available in S3?

BUGS:

As of Sept 22, 2008:
--You can copy from S3 to your local computer, and get the file properties.
And you can copy files into S3.  At the moment copying seems only to work
from the command prompt, and only when you do a dir on the bucket:

(start winfuse after it has been properly configured)
dir \\s3\bucketname
copy \\s3\bucketname\filename c:\localpath\localname 
or
copy c:\localpath\localname \\s3\bucketname\filename 


The reason for this is that the filesystem is constantly looking that directories
and files. When they are local the time to look at a file is small. But when we need
to send out a request to S3 and parse the XML every time we inspect a file, things get
bogged down. So when you perform a directory listing the contents are saved for
future use.  If you try to copy a file and have not performed a dir, the directory
contents are blank or invalid.

--You cannot change the ACL of a file. You cannot create a subdirectory in your bucket.

--Theoretically you could copy files from one bucket to another, but I have not tried it.

--Subdirectories in S3 are not handeled properly, WinS3FS tries to download the
file named "directory/filename" 

--If the bucket name exceeds Windows maximum for SMB shares (15 characters), the
bucket name is truncated in the \\s3 directory listing, but the full name needs
to be provided to cd into it.  But everything works ok after that.

--As the ThreeSharp library does not allow byte ranges of a file to be
downloaded, the entire file is downloaded at once to a temporary file on the
local computer.  Then that temp file is passed on to the filesystem half which
copies the file to the destination intended.  This wreaks havok with the
"progress bar" since the entire file is retreived from S3 with no intervention
from the computer, and once the file is locally present, the progress bar
progresses, albeit quickly. The the temp file is deleted

Enjoy.

Joel

2008 by Joel Hewitt

THIS SOFTWARE IS PROVIDED "AS IS" AND WITHOUT ANY EXPRESS OR
IMPLIED WARRANTIES, INCLUDING, WITHOUT LIMITATION, THE IMPLIED
WARRANTIES OF MERCHANTIBILITY AND FITNESS FOR A PARTICULAR PURPOSE.
```
