# Day 04 — Introduction to the Cisco IOS Command-Line Interface (CLI)

> **Course:** CCNA 200-301  
> **Topic:** Cisco IOS CLI, command modes, basic device configuration, and configuration management  
> **Objective:** Learn how to access a Cisco device, navigate its CLI, enter global configuration mode, configure basic passwords, and save configuration changes.

---

## 1. What is the Cisco IOS CLI?

The **Command-Line Interface (CLI)** is a text-based interface used to configure, monitor, and troubleshoot Cisco networking devices such as routers and switches.

Administrators enter commands through a terminal application. The device interprets each command and performs the requested operation.

### CLI vs. GUI

| CLI | GUI |
|---|---|
| Commands are entered as text | Actions are performed using graphical controls |
| Efficient for repetitive configuration | Often easier for beginners |
| Supports automation and scripting | Automation may be more limited |
| Commonly used by network engineers | Commonly used for visual administration |

The CLI is especially important in networking because it provides direct access to device configuration and operational information.

## 2. Accessing a Cisco Device

A Cisco device can commonly be accessed through:

- **Console connection:** Direct local access, commonly used for initial setup and recovery.
- **SSH:** Secure remote command-line access over an IP network.
- **Telnet:** Remote CLI access, but it sends data without encryption and should generally be avoided in production.

### Console connection

A PC can connect to a router or switch using a console cable and a terminal emulator such as PuTTY, Tera Term, or a terminal application.

Common serial terminal settings:

| Setting | Value |
|---|---:|
| Speed (baud rate) | 9600 |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |
| Flow control | None |

These settings are commonly represented as **9600 8-N-1**.

A console connection does not require the device to have an IP address or a functioning network connection.

## 3. Understanding Cisco CLI Modes

Cisco IOS uses different command modes. The prompt indicates the current mode and determines which commands are available.

### A. User EXEC mode

- Prompt: `Router>`
- The initial mode after connecting to the device.
- Provides limited monitoring and basic operational commands.
- Does not allow full device configuration.

### B. Privileged EXEC mode

- Prompt: `Router#`
- Provides access to advanced monitoring, troubleshooting, and configuration-management commands.
- Enter it from User EXEC mode using:

```text
Router> enable
Router#
```

The command may be abbreviated as `en` when the abbreviation is unambiguous.

### C. Global configuration mode

- Prompt: `Router(config)#`
- Used to make changes that affect the device globally, such as changing its hostname or configuring password settings.

**Command to enter global configuration mode:**

```text
Router> enable
Router# configure terminal
Router(config)#
```

The command can be abbreviated as:

```text
Router# conf t
```

You must first enter Privileged EXEC mode before using `configure terminal`.

### D. Moving between modes

| Command | Purpose |
|---|---|
| `enable` | User EXEC → Privileged EXEC |
| `configure terminal` | Privileged EXEC → Global configuration |
| `exit` | Move back one level or leave the current session |
| `end` | Return directly to Privileged EXEC mode from configuration mode |
| `Ctrl + Z` | Shortcut to return to Privileged EXEC mode |
| `disable` | Privileged EXEC → User EXEC |

Example:

```text
Router> enable
Router# configure terminal
Router(config)# exit
Router#
```

## 4. Getting Help and Discovering Commands

Cisco IOS includes context-sensitive help. The question mark (`?`) displays available commands or possible completions based on the current position.

### Examples

Show commands available in the current mode:

```text
Router> ?
```

Show commands beginning with a particular word:

```text
Router# show ?
```

Show possible next keywords or arguments:

```text
Router(config)# hostname ?
```

If a command is incomplete or ambiguous, IOS can indicate what additional input is required.

### Tab completion

Press **Tab** to complete a command when the characters entered uniquely identify it.

For example, typing `conf` and pressing Tab can complete it to `configure` if no other command creates ambiguity in that context.

### Command abbreviation

Commands can be shortened as long as the abbreviation uniquely identifies the intended command.

Examples:

| Full command | Possible abbreviation |
|---|---|
| `enable` | `en` |
| `configure terminal` | `conf t` |
| `show running-config` | `show run` |
| `copy running-config startup-config` | `copy run start` |

For learning and documentation, prefer full commands. Abbreviations are useful for speed once their meaning is understood.

## 5. Basic Device Configuration

### Set the device hostname

The hostname identifies a router or switch in the CLI prompt.

Enter global configuration mode and set a hostname:

```text
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)#
```

The prompt changes immediately after the hostname is configured.

### Configure an enable password

The `enable password` command sets a password for access to Privileged EXEC mode.

```text
R1(config)# enable password Cisco123
```

This is a legacy method and stores the password in a less secure form unless password encryption is configured. Avoid using weak or reused passwords.

### Configure an enable secret

The recommended basic IOS command for protecting Privileged EXEC access is:

```text
R1(config)# enable secret StrongPassword
```

The `enable secret` takes precedence over `enable password` when both are configured. It stores a password hash rather than the plain-text password.

Use a unique, strong password in a real environment. The example above is for syntax demonstration only.

### Encrypt legacy plain-text passwords in the configuration

The global configuration command:

```text
R1(config)# service password-encryption
```

obfuscates certain plain-text passwords in the configuration file. This is not strong modern cryptographic protection; it should not be treated as a replacement for secure password design or stronger authentication controls.

## 6. Viewing Device Configuration

Two important configuration types are stored on a Cisco device.

### Running configuration

- The active configuration currently used by the device.
- Stored in RAM.
- Changes take effect as commands are applied.
- Lost after a restart if they have not been saved to startup configuration.

View it with:

```text
R1# show running-config
```

Common abbreviation:

```text
R1# show run
```

### Startup configuration

- The configuration loaded when the device boots.
- Stored in non-volatile memory (NVRAM on many classic Cisco platforms).
- Remains available after a reboot.

View it with:

```text
R1# show startup-config
```

### Running vs. startup configuration

| Running configuration | Startup configuration |
|---|---|
| Active configuration | Saved boot configuration |
| Stored in RAM | Stored in non-volatile storage |
| Changes are applied here first | Updated when configuration is saved |
| Can be lost on reboot if not saved | Used during the next boot |

## 7. Saving Configuration Changes

After making configuration changes, save the running configuration so that it is retained after a restart.

The standard command is:

```text
R1# copy running-config startup-config
```

Common abbreviations:

```text
R1# copy run start
```

Other commonly supported forms include:

```text
R1# write memory
R1# write
```

The exact supported commands can vary by Cisco platform and IOS version. The explicit `copy running-config startup-config` command is clear and widely taught.

**Important:** If you configure a device and forget to save the running configuration, those changes may be lost when the device restarts.

## 8. Useful CLI Commands and Behaviours

| Command / key | Function |
|---|---|
| `?` | Context-sensitive help |
| `Tab` | Complete an unambiguous command |
| `show history` | Display recently entered commands |
| `show running-config` | Display active configuration |
| `show startup-config` | Display saved startup configuration |
| `exit` | Leave the current mode or session |
| `end` | Return to Privileged EXEC mode |
| `Ctrl + Z` | Return to Privileged EXEC mode |
| `do` | Run an EXEC command from configuration mode |
| `no` | Negate or remove a configuration command, where supported |

### The `do` command

The `do` prefix allows many EXEC-level commands to be run while in configuration mode.

Example:

```text
R1(config)# do show running-config
```

This is useful when you want to verify information without leaving global configuration mode.

### The `no` command

The `no` form is commonly used to remove or disable a configuration setting.

For example, to remove a hostname setting and restore the default hostname:

```text
R1(config)# no hostname
```

The exact effect of `no` depends on the command being negated.

## 9. Complete Basic Configuration Walkthrough

The following sequence demonstrates a basic configuration workflow on a Cisco IOS device.

```text
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# enable secret StrongPassword
R1(config)# service password-encryption
R1(config)# end
R1# show running-config
R1# copy running-config startup-config
```

What this sequence does:

1. Enters Privileged EXEC mode.
2. Enters Global Configuration mode.
3. Changes the device hostname to R1.
4. Configures an enable secret.
5. Enables obfuscation of eligible plain-text passwords.
6. Returns to Privileged EXEC mode.
7. Verifies the active configuration.
8. Saves the configuration for the next boot.

## 10. Common Beginner Mistakes

- **Trying to configure from User EXEC mode:** Enter `enable` first.
- **Using `configure terminal` in the wrong mode:** It must be entered from Privileged EXEC mode.
- **Confusing the two configurations:** Running configuration is active; startup configuration is saved for boot.
- **Forgetting to save:** Use `copy running-config startup-config` after making changes.
- **Assuming `service password-encryption` provides strong security:** It is only basic obfuscation for certain passwords.
- **Using ambiguous abbreviations:** Use more characters or the full command.
- **Ignoring the prompt:** The prompt tells you which mode you are in.

## 11. Quick Revision

| Concept | Remember |
|---|---|
| Cisco IOS CLI | Text-based device management interface |
| Console | Local access for setup and recovery |
| User EXEC | `Router>` |
| Privileged EXEC | `Router#` |
| Global configuration | `Router(config)#` |
| Enter Privileged EXEC | `enable` |
| Enter global configuration | `configure terminal` or `conf t` |
| Context help | `?` |
| Command completion | `Tab` |
| Active configuration | Running configuration |
| Saved boot configuration | Startup configuration |
| Save configuration | `copy running-config startup-config` |
| Stronger enable password method | `enable secret` |

## 12. Practice Questions

1. What is the purpose of the Cisco IOS CLI?
2. Which CLI mode displays the prompt `Router>`?
3. Which command moves from User EXEC mode to Privileged EXEC mode?
4. **Which command enters Global Configuration mode?**
5. What does the prompt `Router(config)#` indicate?
6. What is the purpose of the question mark (`?`)?
7. What is the difference between running configuration and startup configuration?
8. Which command saves the running configuration to startup configuration?
9. What is the difference between `enable password` and `enable secret`?
10. What does `service password-encryption` do, and why is it not sufficient as a modern security control?

### Essential command to remember

```text
Router> enable
Router# configure terminal
Router(config)#
```

**Answer:** Use `enable` to enter Privileged EXEC mode, then `configure terminal` (or `conf t`) to enter Global Configuration mode.

---

*Study note prepared for CCNA 200-301 learning and practical Cisco IOS CLI revision. Commands and available features may differ slightly across IOS versions and device platforms.*
