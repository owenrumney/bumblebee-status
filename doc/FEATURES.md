# Features and advanced usage

## Events
Many modules provide default handling for various events, for example:

- Mouse-Wheel on any module moves to the next/previous i3 workspace
- Left-click on the "disk" module opens the specified path in a file browser
- Left-click on either "memory" or "cpu" opens a system monitor
- Left-click on a "pulseaudio" (or pasource/pasink) module toggles the mute state
- Mouse-Wheel up/down on a "pulseaudio" module raises/lowers the volume

You can provide your own handlers to any module by using the following "special" configuration parameters:

- left-click
- right-click
- middle-click
- wheel-up
- wheel-down
For example, to execute "pavucontrol" whenever you left-click on the nic module, you could write:

`$ bumblebee-status -p nic.left-click="pavucontrol"`

In the string, you can use the following format identifiers:
- name
- instance
- button

For example:

`$ bumblebee-status -p disk.left-click="nautilus {instance}"`

## Update intervals
The general "update interval" is set using the `-i|--interval` parameter of `bumblebee-status`, and defaults to 1 second.
Some modules override this interval to update less frequently (e.g. the kernel version is updated only very rarely, as it
usually doesn't change during runtime).
Also, modules like `weather` or `stock` update less frequently, to avoid hitting API limits.

For each module, it is possible to specify a parameter `interval` to override that behaviour.

For example, to update the battery status once per minute, you'd use something like this:

`$ bumblebee-status -m battery -p battery.interval=1m`

The format supports:
- numbers (assumed to be seconds - `battery.interval=20` means every 20s)
- `h`, `m`, `s` and combinations thereof - `battery.interval=2m30s` means every 2 minutes, 30 seconds)

## Errors
If errors occur, you should see them in the i3bar itself. If that does not work, or you need more information for troubleshooting, you can activate a debug log using the `-d` or `--debug` switch:

```
$ ./bumblebee-status -d -m <list of modules>
```

This will log to stderr, so unless you are running `bumblebee-status` interactively in the CLI, you'll need to redirect stderr to some file (i.e. `bumblebee-status <parameters> 2> error.log`).

## Automatically hiding modules
If you want to have a minimal bar that stays out of the way, you can use the `-a` or `--autohide` switch to specify a list of module names. All those modules will only be displayed when (and as long as) their state is either warning or critical (high CPU usage, low disk space, etc.). As long as the module is in a "normal" state and does not require attention, it will remain hidden.
Note that this parameter is specified *in addition* to `-m` (i.e. to autohide the CPU module, you would use `bumblebee-status -m cpu memory traffic -a cpu`).
