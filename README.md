# execnotify

Runs a command and signals the start and end as a notifications

## Usage

    execnotify [ -t title ] <command>
	
`execnotify` takes an optional `-t` option and argument and considers the rest of the arguments as a command that should be run.

It will create (desktop) notifications for the start and end using a configurable notifer.

## Example Usage

A typical usecase is when you run some kind of watcher in the background and you want to be made aware when it starts and finishes.

    watchexec -e c,h -- execnotify -t Unittests make unit
	
This example uses [https://github.com/watchexec/watchexec](watchexec) to watch for file changes in files with extension `.c` or `.h` and when it sees that runs `make unit`.
`execnotify` tells you, with a desktop notification of your choice, when that starts and ends and what the result was.
Perfect for automatically running your unittest when you save "many more much smaller changes".

## Configuration

The default notifier is `terminal-notifier` (MacOS) but you can change that by setting the following variables in `~/.execnotifyrc`:

- notify_start
- notify_pass
- notify_fail

They should be complete commands to execute.
The lines in the script itself can be used as examples.
If the value of the optional `-t` (title) option is to be used in your commands you should use the `$title` shell variable in them. E.g.

    notify_start="$notifier -i $start_icon $title Starting"

`~/.execnotifyrc` is read on every invocation.

Depending on the return code of the command, `execnotifier` will run either `notify-pass` or `notify-fail`.

You can also override the icons used by defining

- start_icon
- pass_icon
- fail_icon

They should be paths to icons suitable for the notifier used. And of course readable by whatever notifier you are using.

### Example `.execnotifyrc`

```
notifier=notify-send

ICONDIR=<directory>
start_icon="$ICONDIR/start-icon.png"
pass_icon="$ICONDIR/pass-icon.png"
fail_icon="$ICONDIR/fail-icon.png"

notify_start="$notifier -i $start_icon $title Starting"
notify_pass="$notifier -i $pass_icon $title Passed"
notify_fail="$notifier -i $fail_icon $title Failed"
```

## TODO

- allow some settings to be defined in the command rather than in your global `.execnotifyrc`, e.g. the title
