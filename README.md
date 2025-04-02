# mpvpaper_slideshow

A seamless wallpaper slideshow script for Wayland compositors using mpvpaper. This script supports both images and videos as wallpapers with smooth transitions and multiple monitor configurations.

## Features

- Support for multiple wallpaper directories
- Image and video wallpapers with configurable weights
- Individual wallpapers per monitor or shared wallpapers
- Smooth transitions between wallpapers
- Forward and backward navigation through wallpaper history
- Detailed logging for troubleshooting
- Compatible with Hyprland, Sway, and other wlroots-based compositors

## Requirements

- `mpvpaper`: The core dependency for displaying wallpapers
- `hyprctl` + `jq` or `wlr-randr`: For detecting monitors
- `killall`: For process management

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/mpvpaper_slideshow.git
   ```

2. Make the script executable:
   ```bash
   chmod +x mpvpaper_slideshow.sh
   ```

3. Install dependencies:
   ```bash
   # For Arch Linux
   sudo pacman -S mpvpaper jq
   
   # For other distributions, use the appropriate package manager
   # or build mpvpaper from source: https://github.com/GhostNaN/mpvpaper
   ```

## Basic Usage

1. Edit the script to configure your wallpaper directories and preferences (see Configuration section)

2. Run the script to start the wallpaper slideshow:
   ```bash
   ./mpvpaper_slideshow.sh
   ```

3. To run it in the background:
   ```bash
   ./mpvpaper_slideshow.sh &
   ```

4. To control the slideshow:
   ```bash
   # Next wallpaper
   kill -SIGUSR1 $(cat /tmp/mpvpaper_slideshow_pid)
   
   # Previous wallpaper
   kill -SIGUSR2 $(cat /tmp/mpvpaper_slideshow_pid)
   ```

5. To stop the slideshow:
   ```bash
   kill $(cat /tmp/mpvpaper_slideshow_pid)
   ```

## Configuration Options

The script's behavior can be customized by editing the following settings at the top of the script:

### Wallpaper Sources

```bash
wallpaperDirs=(
    "/home/chris/Pictures/newpc/mikunewgen/"
    # Add more directories as needed
)
```
- Add multiple directories by adding more entries to the array
- Each directory should contain image or video files

### Supported File Types

```bash
imageExts=("jpg" "jpeg" "png" "bmp" "gif")
videoExts=("mp4" "mkv" "webm" "avi")
```
- Add or remove file extensions as needed

### Content Type Weights

```bash
videoWeight=60
imageWeight=40
```
- Adjusts the relative chance of selecting videos vs images
- Must add up to 100

### Monitor Configuration

```bash
separate_images=1  # 1 = different image on each monitor, 0 = same image on all monitors
separate_videos=0  # 1 = different video on each monitor, 0 = same video on all monitors
```

### Video Options

```bash
videoMute=1        # 1 = mute videos, 0 = play audio
videoVolume=100    # Volume level (1-100) if videos are unmuted
```

### Timing Options

```bash
transitionInterval=240  # Time in seconds before switching to next wallpaper
cleanupDelay=3          # Delay in seconds before killing old mpvpaper instances
```

### Display Options

```bash
scalingOptions="--panscan=1.0 --keepaspect"  # mpv options for scaling content
```

## System Files

The script uses several temporary files:

```bash
historyFile="/tmp/mpvpaper_history"  # Stores wallpaper history
pidFile="/tmp/mpvpaper_slideshow_pid"  # Stores PID for external control
logFile="/tmp/mpvpaper_slideshow.log"  # Stores detailed logs
```

## Advanced Usage

### Autostart with Hyprland

Add to your `~/.config/hypr/hyprland.conf`:

```
exec-once = /path/to/mpvpaper_slideshow.sh
```

### Creating Keybindings (Hyprland)

Add to your `~/.config/hypr/hyprland.conf`:

```
bind = $mainMod, right, exec, kill -SIGUSR1 $(cat /tmp/mpvpaper_slideshow_pid)  # Next wallpaper
bind = $mainMod, left, exec, kill -SIGUSR2 $(cat /tmp/mpvpaper_slideshow_pid)   # Previous wallpaper
```

## Troubleshooting

If you encounter issues:

1. Check the log file:
   ```bash
   cat /tmp/mpvpaper_slideshow.log
   ```

2. Make sure mpvpaper is installed and functioning:
   ```bash
   mpvpaper --help
   ```

3. Verify that your monitor detection method works:
   ```bash
   hyprctl -j monitors  # For Hyprland
   # OR
   wlr-randr            # For other wlroots compositors
   ```

## License

This project is licensed under the MIT License - see the LICENSE file for details.
