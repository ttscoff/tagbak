# TagBak

A command line utility for saving and restoring OS X tag information

---

TagBak can store tags for all files in the current folder and all subfolders. It can then restore the tags to the state they were in at the last run. Files which had existing tags will have any current tags replaced. Files that didn't have tags at the time of the run but have since been tagged will be left with their new tags.

TagBak is intended for use with services that do not currently preserve tag data. If you run a remote backup, for instance, and the service strips tags, you can run TagBak prior to a backup and have the metadata for the backed-up files stored with them. Upon restore, TagBak can read the metadata file and restore the state of the tags as they were at the time of backup.

## Installation

Put the `tagbak` script in a folder in your PATH, such as `/usr/local/bin/`. Make it executable by running `chmod a+x /path/to/tagbak`. Optionally install the `bookmark` utility (see below).

## Usage

`tagbak store` will create a `.metadata.stash` file in the current directory (or one specified with an argument, e.g. `tagbak store ~/Dropbox`). Unless the `-s` switch is given, a progress readout will show the current status of the storage task. On my MacBook Air, it takes about 10 seconds per 100 files and the resulting file is about 10k per 100 files.

`tagbak restore` will find the nearest `.metadata.stash` file, looking up the folder tree if necessary, and restore the tags from the current folder down the tree.

`tagbak info` will give you a file list, total bookmarks, and file size for the neartest `.metadata.stash` file.

## Additional command line options

    Usage: tagbak [options] (store|restore|info) [dir [dir...]]
        -s, --silent                     Run silently
            --cli_util PATH              Use alternate location for bookmark tool (default /usr/local/bin/bookmark)
            --ignore_pattern PATTERN     File pattern (regex) to ignore (default "\.(git|svn)\/.*$")
            --debug LEVEL                Degug level [1-3] (default 1)
            --stash_name FILENAME        Use alternate stash file name (default .metadata.stash)
        -h, --help                       Display this screen

## bookmark-cli

If the [bookmark utility](https://github.com/ttscoff/bookmark-cli) is installed in `/usr/local/bin/bookmark`, TagBak will store bookmark information with the metadata. This makes it possible to restore tags on files that have moved or been renamed, but only within the local drive and only if their file data hasn't changed. Restoring from a backup service or an external drive will destroy this data, so it's only a valid precaution in a few cases. Storing this data doesn't cause any major slowdown and the resulting stash sizes are still of a very manageable size, so it doesn't really hurt to include it.
