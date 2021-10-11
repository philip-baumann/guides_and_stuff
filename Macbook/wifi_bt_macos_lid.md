# Disable and enable WiFi and Bluetooth on lid close/open
## Prerequisites
* [Homebrew](https://brew.sh)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

* [Sleepwatcher](https://formulae.brew.sh/formula/sleepwatcher)
```bash
brew install sleepwatcher && brew services start sleepwatcher
```
When starting the service you should be prompted by MacOS to allow the service to be run automatically.

* [blueutil](https://formulae.brew.sh/formula/blueutil)
```bash
brew install blueutil
```

## Configuration
Sleepwatcher looks for user scripts named `~/.sleep` and `~/.wakeup` and system scripts named `/etc/rc.sleep` and `/etc/rc.wakeup`.

We'll be using the user scripts for our example.

### Permissions
Scripts must be executable by the creator/user, so we need to execute `chmod`:
```bash
chmod 700 ~/.sleep
chmod 700 ~/.wakeup
```

### Scripts
The scripts I'm using can be found under [Scripts/Bash/sleepwatcher](Scripts/Bash/sleepwatcher).

#### .sleep
```bash
#!/usr/bin/env bash
networksetup -setairportpower en0 off
blueutil -p 0
```

#### .wakeup
```bash
#!/usr/bin/env bash
sleep 10
networksetup -setairportpower en0 on
blueutil -p 1
```

## Testing
... just... just close and open the lid..?

To _explicitly_ test the scripts, we can use sleepwatcher directly:
```bash
sleepwatcher --verbose --sleep ~/.sleep
```
Enable WiFi and BT, if not already active, and close the lid to test the sleep trigger.
```bash
sleepwatcher --verbose --wake ~/.wakeup
```
Manually disable WiFi and BT, if not already disabled, and close and re-open the lid to test the wake trigger.

# Further reading
* https://www.kodiakskorner.com/log/258
