# GNOME Configuration Restore Guide

This guide provides instructions for restoring your GNOME desktop environment configurations using dconf.

---

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Backing Up Current Settings](#backing-up-current-settings)
3. [Restoring GNOME Configurations](#restoring-gnome-configurations)
4. [Verifying Restored Settings](#verifying-restored-settings)
5. [Troubleshooting](#troubleshooting)

---

## Prerequisites

- GNOME desktop environment installed
- dconf utility (usually pre-installed with GNOME)
- Access to the terminal
- The `gnome-settings.txt` configuration file in this directory

---

## Backing Up Current Settings

Before restoring configurations, it's recommended to back up your current settings:

```bash
# Back up all GNOME settings
dconf dump / > ~/gnome-settings-backup.txt

# Back up specific settings (optional)
dconf dump /org/gnome/desktop/ > ~/gnome-desktop-backup.txt
```

This creates a backup file that you can use to restore your current settings if needed.

---

## Restoring GNOME Configurations

### Method 1: Restore All Settings

To restore all GNOME configurations from the `gnome-settings.txt` file:

```bash
# Navigate to the gnome directory
cd ~/linux/gnome

# Load all settings from the configuration file
dconf load / < gnome-settings.txt
```

> **⚠️ Warning:** This will replace all your current GNOME settings with the ones from the file. Make sure you have a backup!

### Method 2: Restore Specific Settings

If you only want to restore specific settings, you can target individual paths:

```bash
# Restore only desktop interface settings
dconf load /org/gnome/desktop/interface/ < gnome-settings.txt

# Restore only shell settings
dconf load /org/gnome/shell/ < gnome-settings.txt

# Restore only keybindings
dconf load /org/gnome/desktop/wm/keybindings/ < gnome-settings.txt
```

### Method 3: Manual Configuration Review

For more control, you can review and apply settings manually:

```bash
# View the configuration file
cat gnome-settings.txt

# Apply specific settings using dconf write
dconf write /org/gnome/desktop/interface/color-scheme "'prefer-dark'"
dconf write /org/gnome/desktop/interface/gtk-theme "'Orchis'"
```

---

## Verifying Restored Settings

After restoring the configurations, verify that the settings have been applied:

### 1. Check Specific Settings

```bash
# Check the current color scheme
dconf read /org/gnome/desktop/interface/color-scheme

# Check the GTK theme
dconf read /org/gnome/desktop/interface/gtk-theme

# Check enabled shell extensions
dconf read /org/gnome/shell/enabled-extensions
```

### 2. Visual Verification

1. **Log out and log back in** to ensure all settings take effect
2. Check the appearance settings: `Settings > Appearance`
3. Verify keyboard shortcuts: `Settings > Keyboard > Keyboard Shortcuts`
4. Check enabled extensions: Open `Extensions` app

### 3. Export Current Settings for Comparison

```bash
# Export current settings to compare with the original
dconf dump / > ~/current-settings.txt

# Compare the files
diff gnome-settings.txt ~/current-settings.txt
```

---

## Troubleshooting

### Issue: dconf Command Not Found

Install dconf:

```bash
# For Fedora/RHEL
sudo dnf install dconf

# For Ubuntu/Debian
sudo apt install dconf-cli

# For Arch Linux
sudo pacman -S dconf
```

### Issue: Settings Not Applied After Restore

1. **Restart GNOME Shell:**
   - Press `Alt + F2`
   - Type `r` and press Enter
   - Or log out and log back in

2. **Check for errors:**
   ```bash
   # Verify the configuration file format
   dconf load / < gnome-settings.txt
   ```
   
   Look for any error messages that indicate syntax issues.

### Issue: Some Extensions Not Loading

Extensions listed in the configuration may need to be installed first:

```bash
# Install extensions from GNOME Extensions website
# or use gnome-extensions command if available

# List currently installed extensions
gnome-extensions list

# Enable an extension
gnome-extensions enable extension-name@extension-id
```

Common extensions from the configuration:
- `blur-my-shell@aunetx`
- `clipboard-indicator@tudmotu.com`
- `hidetopbar@mathieu.bidon.ca`
- `tiling-assistant@leleat-on-github`
- `switcher@landau.fi`

### Issue: Reset to Default Settings

If you need to reset GNOME to default settings:

```bash
# Reset all GNOME settings
dconf reset -f /org/gnome/

# Or reset specific paths
dconf reset -f /org/gnome/desktop/interface/
```

Then restore from your backup:

```bash
dconf load / < ~/gnome-settings-backup.txt
```

---

## Additional Tips

### Selective Configuration Import

You can create custom configuration files for specific components:

```bash
# Export only shell settings
dconf dump /org/gnome/shell/ > shell-settings.txt

# Export only desktop settings
dconf dump /org/gnome/desktop/ > desktop-settings.txt

# Import them individually later
dconf load /org/gnome/shell/ < shell-settings.txt
```

### Monitoring Configuration Changes

Watch for configuration changes in real-time:

```bash
# Monitor all changes
dconf watch /

# Monitor specific path
dconf watch /org/gnome/desktop/interface/
```

This is useful when you want to see what settings change when you modify something in the GUI.

---

## Additional Resources

- **dconf Documentation:** [https://wiki.gnome.org/Projects/dconf](https://wiki.gnome.org/Projects/dconf)
- **GNOME Shell Extensions:** [https://extensions.gnome.org/](https://extensions.gnome.org/)
- **GNOME Documentation:** [https://help.gnome.org/](https://help.gnome.org/)

---

**Congratulations!** 🎉 Your GNOME desktop environment is now configured with your preferred settings!
