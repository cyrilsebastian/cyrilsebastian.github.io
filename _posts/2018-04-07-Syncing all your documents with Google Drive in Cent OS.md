---
layout: post
title:  "Syncing all your documents with Google Drive in Cent OS."
date:   2018-04-07 00:01:00 +0530
categories: [GoogleDrive, CentOS, clouddrive, sync]
#image: Image name.png
---

There is always a need to sync data, I understand you can easily add all your code to Github but you
often have half baked code or data which can be kept as archived. Also when you don't need to share your
documents publicly. Currently I am using CentOS 7 in my personal laptop and often need to sync few files with my official laptop. But Google do not have any client which support Google Drive sync for your documents.

There are mostly paid cloud sync clients, but as an alternative I found [Rclone](https://rclone.org/drive/) which is developed by [Nick Craig-Wood](https://github.com/ncw). Rclone can sync files to Google Drive, Google Cloud
Storage, AWS bucket, Dropbox, Microsoft Azure, Onedrive etc. This is a CLI program which works same as Github
to code, sync all the files to the mentioned drive.

To install Rclone you need to ruining
{% highlight ruby %}
curl https://rclone.org/install.sh | sudo bash
{% endhighlight %}

To configure Rclone you need to follow these steps.
{% highlight ruby %}
No remotes found - make a new one
n) New remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
n/r/c/s/q> n
name> remote
Type of storage to configure.
Choose a number from below, or type in your own value
[snip]
10 / Google Drive
   \ "drive"
[snip]
Storage> drive
Google Application Client Id - leave blank normally.
client_id>
Google Application Client Secret - leave blank normally.
client_secret>
Scope that rclone should use when requesting access from drive.
Choose a number from below, or type in your own value
 1 / Full access all files, excluding Application Data Folder.
   \ "drive"
 2 / Read-only access to file metadata and file contents.
   \ "drive.readonly"
   / Access to files created by rclone only.
 3 | These are visible in the drive website.
   | File authorization is revoked when the user deauthorizes the app.
   \ "drive.file"
   / Allows read and write access to the Application Data folder.
 4 | This is not visible in the drive website.
   \ "drive.appfolder"
   / Allows read-only access to file metadata but
 5 | does not allow any access to read or download file content.
   \ "drive.metadata.readonly"
scope> 1
ID of the root folder - leave blank normally.  Fill in to access "Computers" folders. (see docs).
root_folder_id>
Service Account Credentials JSON file path - needed only if you want use SA instead of interactive login.
service_account_file>
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine or Y didn't work
y) Yes
n) No
y/n> y
If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth
Log in and authorize rclone for access
Waiting for code...
Got code
Configure this as a team drive?
y) Yes
n) No
y/n> n
--------------------
[remote]
client_id =
client_secret =
scope = drive
root_folder_id =
service_account_file =
token = {"access_token":"XXX","token_type":"Bearer","refresh_token":"XXX","expiry":"2014-03-16T13:57:58.955387075Z"}
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
{% endhighlight %}

Once configured with your account by Google Oauth you can start using it and sync data with mentioned
commands below.
{% highlight ruby %}
rclone lsd remote:          #to list Google Drive directories
rclone ls remote:           #to list files present in Google Drive.
rclone copy /home/source remote:backup          #to sync the file to Google Drive's backup directory.
{% endhighlight %}

You can get more details on Rclone's [official documents](https://rclone.org/drive/). As its free to use,
you can also share your feedback on [Github](https://github.com/ncw/rclone/) to make this application better.
While using Rclone, please make sure you copy changes made to your documents so that the documents are synced.
