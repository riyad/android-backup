# ChangeLog

## v0.8.0

* Use `su` instead of `adb root` to prevent errors like "adbd cannot run as root in production builds"
* Recognize there're other (maintained) backup tools (e.g. Seedvault and Neo Backup)
  * Backup/restore their APKs if found
  * Restore their data directories with priority
  * Print instructions for starting the restoration process (for Seedvault)
* Filter out /Android/obb
* Ask for confirmation before actually restoring files
  * Add `--assume-yes` option to android-restore

## v0.7.1

* Improve documentation
* Code quality improvements
* Fix reading device serial number from paths with spaces

## v0.7

* Save device serial number into backup directory and check it before backup or restore

## v0.6

* Change license to MPL-2.0

## v0.5.1

* Add `--assume-yes` option to android-backup

## v0.5

* Add different log levels
* Improve rsync setup
* Check scripts with [ShellCheck](https://github.com/koalaman/shellcheck)
* Add `--debug` option for (very) verbose debugging output
* Add `--use-root` option to make use of root explicit
