# Chapter 3 Solutions: GNOME Desktop

## Exercise Solutions

### Exercise 1: Navigate GNOME Activities

**Task:** Explore the Activities Overview and Dash.

**Solution:**

**Open Activities Overview:**
```
Method 1: Press Super (Windows) key
Method 2: Press Alt + F1
Method 3: Move mouse to top-left "Activities" hot corner
```

**What to explore:**

1. **Application Launcher**:
   - Click grid icon (or press Super+A)
   - Scroll through installed applications
   - Type to search: `fire` → Firefox appears

2. **Workspace Switcher**:
   - Right side of Activities Overview
   - Click workspace to switch
   - Drag windows between workspaces
   - Dynamic workspaces appear as needed

3. **Window Picker**:
   - Shows all open windows
   - Click window to focus
   - Right-click for window actions (close, move to workspace)

**Adding to Favorites:**
```
1. Open Activities Overview
2. Right-click application icon
3. Select "Add to Favorites"
4. Application appears in Dash
```

**Expected Result:**
Favorited applications appear in the Dash (vertical bar on left) for quick access.

---

### Exercise 2: Use Workspaces

**Task:** Create and organize workspaces for different tasks.

**Solution:**

**Creating Workspaces:**
```bash
# Method 1: Open Activities, click + at bottom right
# Method 2: Press Super+PgDn to create new workspace
# Method 3: Drag window to empty workspace area
```

**Organizing Example:**

```
Workspace 1: Web Browsing
- Firefox
- Discord

Workspace 2: Development
- VS Code
- Terminal
- Documentation (Firefox)

Workspace 3: Music/Video
- Spotify
- VLC
```

**Switching Between Workspaces:**
```
Keyboard shortcuts:
- Super+PgUp: Previous workspace
- Super+PgDn: Next workspace
- Super+1-9: Switch to specific workspace
```

**Moving Windows to Workspaces:**
```
1. Open Activities Overview
2. Right-click window
3. "Move to Workspace" → Select workspace
```

**Expected Output:**
Each workspace maintains independent window arrangement. Switching workspace shows only that workspace's windows.

---

### Exercise 3: Install GNOME Extensions

**Task:** Install and configure useful GNOME extensions.

**Solution:**

**Install Extension Manager:**

```bash
# Fedora
sudo dnf install extension-manager

# Debian/Ubuntu
sudo apt install gnome-shell-extensions
```

**Using Extension Manager:**

1. Open Extension Manager
2. Browse featured extensions
3. Toggle extensions on/off

**Installing from Browser:**

1. Install GNOME Shell integration:
```bash
# Fedora
sudo dnf install chrome-gnome-shell gnome-browser-connector

# Debian/Ubuntu
sudo apt install gnome-shell-extensions
```

2. Install browser extension:
   - Firefox: https://addons.mozilla.org/firefox/addon/gnome-shell-integration/
   - Chrome: https://chrome.google.com/webstore/detail/gnome-shell-integration/

3. Visit https://extensions.gnome.org/
4. Browse and install extensions

**Recommended Extensions:**

| Extension | Purpose |
|-----------|---------|
| Dash to Dock | Convert Dash to dock, position it anywhere |
| Caffeine | Prevent screen from automatically sleeping |
| AppIndicator/KStatusNotifierItem | Show tray icons (Discord, etc.) |
| GSConnect | Android phone integration |
| User Themes | Allow custom shell themes |

**Expected Result:**
Extensions appear in Extension Manager and can be toggled. Some extensions require GNOME Shell restart (`Alt+F2`, type `r`, press Enter).

---

### Exercise 4: Customize Appearance

**Task:** Change wallpaper, theme, and fonts.

**Solution:**

**Open Settings:**
```
Press Super → Type "Settings" → Press Enter
Or: Click system menu (top right) → Settings
```

**Change Wallpaper:**
```
1. Settings → Background
2. Choose from default wallpapers
3. Or click "Add Picture..." to use custom image
```

**Enable Dark Mode:**
```
1. Settings → Appearance
2. Toggle "Dark Style" or select "Dark"
```

**Change Accent Color:**
```
1. Settings → Appearance
2. Choose accent color (Blue, Teal, Green, etc.)
```

**Change Fonts:**
```
1. Settings → Fonts
2. Configure:
   - Default Font: Ubuntu Regular 11
   - Monospace Font: Ubuntu Mono 13
   - Document Font: Sans Regular 11
```

**Expected Result:**
GNOME immediately applies changes. All applications respect theme settings.

---

### Exercise 5: Use GNOME Keyboard Shortcuts

**Task:** Practice essential GNOME keyboard shortcuts.

**Solution:**

**Essential Shortcuts Practice:**

| Shortcut | Action | Practice |
|----------|--------|----------|
| Super | Open Activities Overview | Open/close repeatedly |
| Super+A | Show all applications | Try it |
| Alt+Tab | Switch windows | Switch between 2+ windows |
| Super+PgUp/PgDn | Switch workspaces | Create 2 workspaces, switch |
| Super+Enter | Launch terminal (if configured) | Open terminal |
| Super+L | Lock screen | Lock and unlock |
| Alt+F2 | Run command dialog | Type `r` to restart shell |
| Print | Take screenshot | Take a screenshot |

**View All Shortcuts:**
```
1. Settings → Keyboard
2. Scroll through keyboard shortcuts
3. Click on shortcut to customize
```

**Custom Shortcut Example:**

```
1. Settings → Keyboard → View and Customize Shortcuts
2. Scroll to "Custom Shortcuts"
3. Click + button
4. Name: "Launch Firefox"
5. Command: firefox
6. Shortcut: Ctrl+Alt+F
```

**Expected Result:**
Pressing the custom shortcut launches Firefox. Custom shortcuts appear in the keyboard settings.

---

## Common GNOME Customizations

### Enable minimized window to be iconified on Dash

```bash
# Install Dash to Dock extension
# Settings → Extensions → Dash to Dock
# Configure: "Intelligent hide" or "Auto hide"
```

### Add minimize/maximize buttons to windows

```bash
# Install GNOME Tweaks
sudo dnf install gnome-tweaks    # Fedora
sudo apt install gnome-tweaks    # Debian

# Open GNOME Tweaks
# Window Titlebars → Placement → Left
# Window Titlebars → Maximize/Minimize → Enabled
```

### Change power button behavior

```
1. Settings → Power
2. Power Button Behavior → "Suspend" or "Power Off"
```

### Auto-hide GNOME Top Bar

```bash
# Install "Hide Top Bar" extension
# Configure auto-hide delay
```

---

## Instructor Notes

### Teaching Tips

1. **Live Demonstration**: Show GNOME features in real-time during lecture
2. **Keyboard Shortcut Challenge**: Have students race to complete tasks using only shortcuts
3. **Extension Safety**: Warn students that poorly-written extensions can cause crashes
4. **Customization Philosophy**: Emphasize that GNOME works great out of the box - customize thoughtfully
5. **Troubleshooting**: Show how to disable extensions that cause problems

### Common Student Issues

| Issue | Solution |
|-------|----------|
| Extensions don't work | GNOME Shell version must match extension version |
| Can't find installed app | Type name in Activities search |
| Lost in workspaces | Press Super to return to overview |
| Wonky screen layout | Reset to defaults: `gsettings reset-recursively org.gnome.shell` |
| Accidentally deleted favorite | Right-click Dash → "Show Details" → Add back |

### Extension Activities

- Set up optimal workspace layout for development
- Create custom keyboard shortcuts for common tasks
- Design a personalized GNOME theme
- Compare GNOME with KDE Plasma or XFCE
- Explore GNOME settings not covered in class
- Set up multiple user accounts with different configurations
