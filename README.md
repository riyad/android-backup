Android-Backup
==============

Backup and restore your Android phone with ADB (and rsync)

It will backup and restore all of your `/sdcard` directory and any other storage (e.g. an external SD Card) mounted within `/storage` except for `emulated` and `self`).
Assuming you're using also something like [Neo Backup](https://github.com/NeoApplications/Neo-Backup) you'll be able to backup and restore all your apps, settings and data.

It uses [ADB](https://developer.android.com/tools/help/adb.html) for setup and [rsync](https://en.wikipedia.org/wiki/Rsync) to do the copying since the [Android File Transfer Tool](https://www.android.com/filetransfer/) for Mac has a laughable quality for Google’s standards.

It's based on a [pure ADB version](https://github.com/riyad/android-backup/releases/tag/adb-pull-push-only) by Riyad Preukschas and has been improved with ideas and methods from [Simon Josefsson](https://blog.josefsson.org/2015/11/28/automatic-android-replicant-backup-over-usb-using-rsync/) and [pts](http://ptspts.blogspot.de/2015/03/how-to-use-rsync-over-adb-on-android.html).

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

If you have an older backup already you can use it to speed up the backup process. Rsync will automatically find out what files to download leaving out those that have not changed in the meantime.

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

### Without Root

* Currently restoring will fail to restore the correct timestamps of files.
  They'll be set to the date and time of when you're restoring them.
  This will cause all of those files to be downloaded again for the first backup, because it will look as if those files were updated.

* During data transfer you may see errors like `rsync: readlink_stat("/path/to/some/file" (in root)) failed: Permission denied (13)` followed later by `rsync: recv_generator: failed to stat "/path/to/some/file" (in root): Permission denied (13)`.
  The files seem to still get restored fine (with the caveat above).
  Otherwise please use the contact information below.

## Extract Rsync Binary From Android

The provided rsync.bkp file was extracted from LineageOS 14.1-20181110-NIGHTLY for OnePlus X (i.e. Android 7.1.2; API 25; armeabi-v7a). This will work on newer Android versions as well as arm64-v8a ABI, but not the other way round.

You can do this yourself via:

```shell
adb shell which rsync
adb pull /system/xbin/rsync rsync.bkp
```

## Build Rsync for Android Yourself

NOTE: The following steps assume you already have the Android NDK installed.

Clone the rsync for android source (e.g. from @LineageOS) ...

```shell
git clone https://github.com/LineageOS/android_external_rsync.git
cd android_external_rsync
# checkout the most recent branch
git checkout cm-18.1
```

... create the missing `Application.mk` build file ...

```
mkdir jni
# create missing build script (e.g. from https://gist.github.com/riyad/59c17ce7a1ade6cfc3c6)
wget -O jni/Application.mk https://gist.githubusercontent.com/riyad/59c17ce7a1ade6cfc3c6/raw/6a0c6aa9c8a4273ad304085f4e568ceedfbe0b48/Application.mk
```

... and start the build with:

```shell
export NDK_PROJECT_PATH=`pwd`
ndk-build -d rsync
```

Find your self-build rsync in `obj/local/*/rsync`.

## Tested With

* Nexus 5, LineageOS 14.1 (Android 7.1.2)
* OnePlus X, LineageOS 14.1 (Android 7.1.2)
* Samsung Galaxy S9, LineageOS 17.1 (Android 10)
* Google Pixel 6, Graphene OS (Android 13)

## Contact and Issues

Please, report all issues on our issue tracker on GitHub: https://github.com/riyad/android-backup/issues
