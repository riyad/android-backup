Android-Backup
==============

Backup and restore your Android phone with ADB (and rsync)

It will backup and restore all of your `/sdcard` directory.
Assuming you're using also something like [Titanium Backup](https://play.google.com/store/apps/details?id=com.keramidas.TitaniumBackup) you'll be able to backup and restore all your apps, settings and data.

It uses [ADB](https://developer.android.com/tools/help/adb.html) for setup and [rsync](https://en.wikipedia.org/wiki/Rsync) to do the copying since the [Android File Transfer Tool](https://www.android.com/filetransfer/) for Mac has a laughable quality for Google’s standards.

It's based on a [pure ADB version](https://gist.github.com/riyad/d7977d0ba432f21bd0bf) by Riyad Preukschas and has been improved with ideas and methods from [Simon Josefsson](https://blog.josefsson.org/2015/11/28/automatic-android-replicant-backup-over-usb-using-rsync/) and [pts](http://ptspts.blogspot.de/2015/03/how-to-use-rsync-over-adb-on-android.html).

## Installation

Assuming you've installed and setup [ADB](https://developer.android.com/tools/help/adb.html) from [Android's SDK tools](https://developer.android.com/sdk/index.html) package.
Download and extract the current version.

```shell
wget -O android-backup-master.tar.gz https://github.com/riyad/android-backup/archive/master.tar.gz
tar xzf android-backup-master.tar.gz
cd android-backup-master/
```

Move the files to their necessary locations.

```shell
# move scripts to a directory on $PATH
mv android-backup android-restore /usr/local/bin/
# move backup rsync to a location where it can be found
mkdir /usr/local/lib/android-backup
mv rsync.bkp /usr/local/lib/android-backup/
```

Make sure that the directory you move `android-backup` and `android-restore` to is in `$PATH`.

## Usage

### Backup

Connect your phone via USB.
Make sure you have set up ADB debugging already.
Then run:

```shell
android-backup [<backup-dir>]
```

If you haven't given a backup directory it will ask you if it should generate one for you.

If you have an older backup already you can use it to speed up the backup process. Rsync will automatically find out what files to download leaving out those that have not changed in the mean time.

```shell
cp -r previous-backup/ current-backup/
android-backup current-backup/
```

### Restore

Connect your phone via USB.
Make sure you have set up ADB debugging already.
Then run:

```shell
android-restore <backup-dir>
```

## Caveats

* Currently restoring will not be able to be able to restore the correct timestamps of files. They'll be set to the current date and time when you're restoring them.

## Extract Rsync for Android Yourself

The provided rsync.bkp file was extracted from CyanogenOS 12.1.1 for OnePlus One via

```shell
adb shell which rsync
adb pull /system/xbin/rsync rsync.bkp
```

## Build Rsync for Android Yourself

NOTE: The following steps assume you already have the Android NDK installed.

Clone the rsync for android source (e.g. from @CyanogenMod) ...

```shell
git clone https://github.com/CyanogenMod/android_external_rsync.git
cd android_external_rsync
# checkout the most recent branch
git checkout cm-13.0
```

... create the missing `Application.mk` build file ...

```
mkdir jni
# create missing build script (e.g. from https://gist.github.com/riyad/59c17ce7a1ade6cfc3c6)
wget -O jni/Application.mk https://gist.githubusercontent.com/riyad/59c17ce7a1ade6cfc3c6/raw/b56679f6188d9f56315dcd5e904fad0f9bd1439d/Application.mk
```

... and start the build.

```shell
export NDK_PROJECT_PATH=`pwd`
ndk-build -d rsync
```

Find your self-build rsync in `obj/local/*/rsync`.

## Contact and Issues

Please, report all issues on our issue tracker on GitHub: https://github.com/riyad/android-backup/issues
