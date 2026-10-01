## tbsm

![Screen](screen.png)

TBSM (TUI Boot Selection Menu) is a text-based boot selection menu for choosing desktop environments on TTY.

### Features

- Text-based interface for selecting desktop environments
- **NEW: Wayland session support** (in addition to X11)
- Displays system information (user, kernel, time, distro, uptime)
- Configurable appearance and behavior
- **NEW: PIN login support for added security**
- **NEW: Improved error handling and validation**
- Test mode for previewing the menu

### Usage

By default, TBSM runs on any TTY you start it on, even if an X server is already running. This allows you to switch to a different TTY (e.g., Ctrl+Alt+F2) and start a different desktop environment without logging out of your current session.

To restore the original behavior (only run on VT1 when no X server is running), set `restrictToVT1=1` in the config file.

#### Command Line Options

```bash
# Test mode - preview the menu without starting a session
./tbsm -t
./tbsm --test-mode

# Setup PIN login
./tbsm --setup-pin

# Disable PIN login
./tbsm --disable-pin

# Show help
./tbsm -h
./tbsm --help
```

### Configuration

Configuration is stored in `~/.config/tbsm/config`. The file is automatically created with default values on first run.

#### Configuration Options

```bash
# PIN Login Settings
enablePinLogin=0          # Set to 1 to enable PIN login
userPin=""                # Set your PIN (recommended: numbers only)

# Display Options
showStatus=1              # Show system status
showUserAtHost=1          # Show user@hostname
showLinuxVersion=1        # Show kernel version
showTime=1                # Show current time
showLinuxDistName=1       # Show distribution name
showUptime=1              # Show system uptime

# Session Options
runXinitrc=0              # Run .xinitrc/.xsession if present
enableWayland=1           # Enable Wayland session support
restrictToVT1=0           # Set to 1 to only run on VT1, 0 to run on any TTY
```

### Setting Up PIN Login

#### Method 1: Using the setup command (recommended)
```bash
./tbsm --setup-pin
```
Follow the prompts to enter and confirm your PIN.

#### Method 2: Manual configuration
Edit `~/.config/tbsm/config`:
```bash
enablePinLogin=1
userPin="1234"
```

Replace `1234` with your desired PIN.

### Security Notes

- PIN login provides basic protection against unauthorized access
- The PIN is stored in plain text in the config file
- For stronger security, consider using full disk encryption or a display manager with proper authentication
- After 3 failed PIN attempts, the user is returned to the shell

### Wayland Support

TBSM now supports Wayland sessions alongside X11 sessions. The script automatically:
- Scans `/usr/share/xsessions/` for X11 sessions
- Scans `/usr/share/wayland-sessions/` for Wayland sessions (if enabled)
- Detects the session type and uses the appropriate startup method
- Starts Wayland compositors directly without X11

To disable Wayland support, set `enableWayland=0` in the config file.

### Running on Any TTY

By default, TBSM is configured to run on any TTY, even if an X server is already running. This allows you to:
- Switch to a different TTY (e.g., Ctrl+Alt+F2) and start a different desktop environment
- Test multiple desktop environments without logging out
- Use TBSM alongside an existing X session

When starting a session while another X server is running, TBSM will display a note but continue with the session launch.

To restore the original behavior (only run on VT1 when no X server is running), set `restrictToVT1=1` in the config file.

### Requirements

- Bash
- Linux system with X11
- Desktop environment .desktop files in `/usr/share/xsessions/`

### License

See LICENSE file for details.
