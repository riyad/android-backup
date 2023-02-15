# ChangeLog

## v0.8.0

* use `su` instead of `adb root` to prevent errors like "adbd cannot run as root in production builds"
* recognize there're other (maintained) backup tools (now also for Seedvault and Neo Backup)
  * install APKs if necessary
  * restore their data directories with priority
  * print instructions for starting the restoration process

## v0.7.1

* Improve documentation
* Code quality improvements
* Fix reading device serial number from pathes with spaces

## v0.7

* Save device serial number into backup directory and check it before backup or restore

## v0.6

* Change license to MPL-2.0

## v0.5.1

* Add `--assume-yes` option

## v0.5

* Add different log levels
* Improve rsync setup
* Check scripts with [ShellCheck](https://github.com/koalaman/shellcheck)
* Add `--debug` option for (very) verbose debugging output
* Add `--use-root` option to make use of root explicit
