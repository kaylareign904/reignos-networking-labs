# Lab 04 — Basic Device Security

## Overview

In this lab, I practiced basic Cisco IOS device configuration and security on a router and switch.

I configured device hostnames, created a privileged EXEC password, enabled password encryption, configured a more secure `enable secret`, verified the configuration using `show running-config`, and saved the active running configuration to the startup configuration.

The same configuration tasks were completed on both **R1** and **SW1**. The screenshots included in this documentation show my work on SW1.

This lab also gave me more experience navigating Cisco IOS command modes and troubleshooting commands that did not initially work as expected.

---

## Objectives

By completing this lab, I practiced:

- Navigating Cisco IOS command modes
- Changing router and switch hostnames
- Configuring an `enable password`
- Testing privileged EXEC authentication
- Viewing the running configuration
- Protecting applicable plaintext passwords with `service password-encryption`
- Configuring a more secure `enable secret`
- Comparing password representations in the running configuration
- Executing privileged EXEC commands from Global Configuration mode
- Saving the running configuration to startup configuration
- Troubleshooting Cisco IOS command and prompt errors

---

## Lab Environment

| Component | Details |
|---|---|
| Platform | Cisco Packet Tracer |
| Router | R1 |
| Switch | SW1 |
| Interface | Cisco IOS CLI |
| Main Focus | Basic device configuration and security |

---

## Configuring Device Hostnames

The first task was to change the default device names to the names specified in the lab.

I entered Global Configuration mode and used the `hostname` command.

For the switch:

```text
enable
configure terminal
hostname SW1
```

The same process was performed on the router:

```text
enable
configure terminal
hostname R1
```

![Changing the hostname on SW1](<SW1 changing hostname.png>)

*Changing the switch hostname to SW1.*

Changing the hostname made it easier to identify which device I was configuring because the Cisco IOS command prompt changed to reflect the new device name.

For example:

```text
Switch(config)#
```

became:

```text
SW1(config)#
```

---

## Configuring an Enable Password

Next, I configured an unencrypted privileged EXEC password using:

```text
enable password CCNA
```

![Configuring an unencrypted enable password on SW1](<SW1 enabling unencrypted password.png>)

*Configuring the initial enable password on SW1.*

The same password configuration was also completed on R1.

---

## Testing the Enable Password

After configuring the password, I exited back to User EXEC mode.

I then entered:

```text
enable
```

The device prompted me for the password.

After entering the configured password, I successfully returned to Privileged EXEC mode.

![Testing the enable password on SW1](<SW1 Password enabled successfully.png>)

*Testing the configured password by returning to Privileged EXEC mode.*

This demonstrated that the `enable password` command was successfully protecting access to Privileged EXEC mode.

> **Security Note:** The passwords used in this lab are lab-only credentials provided for the exercise. They are not examples of passwords I would use on a production network.

---

## Viewing the Running Configuration

I used the following command to inspect the active configuration:

```text
show running-config
```

The running configuration allowed me to verify settings such as:

- The configured hostname
- The enable password
- Password encryption settings
- Other active device configuration

![Viewing passwords in the running configuration](<SW1 show running config file to see passwords.png>)

*Reviewing the active running configuration on SW1.*

This helped reinforce that the running configuration represents the configuration currently active on the device.

---

## Enabling Password Encryption

The next task was to ensure that the current password, as well as applicable future passwords, would not appear as plaintext in the configuration.

I used:

```text
service password-encryption
```

![Enabling password encryption on SW1](<SW1 current-future passwords will be encrypted.png>)

*Enabling `service password-encryption` to protect applicable current and future plaintext passwords.*

After enabling this feature, the previously configured `enable password` was no longer displayed as readable plaintext in the running configuration.

Instead, Packet Tracer displayed the password as a Cisco **Type 7** value.

This demonstrated the difference between storing a password directly as plaintext and enabling Cisco IOS password obfuscation.

It also helped me understand that Type 7 encryption is not considered strong password protection. Its primary benefit is preventing a password from being immediately readable when viewing the configuration.

---

## Configuring a More Secure Enable Secret

The next task was to configure a more secure password for Privileged EXEC mode.

I used:

```text
enable secret Cisco
```

![Configuring a more secure enable secret](<SW1 used a more secure password encryption -MD5 secret.png>)

*Configuring an `enable secret` on SW1.*

The same configuration was also completed on R1.

When both an `enable password` and an `enable secret` are configured, the `enable secret` takes precedence when entering Privileged EXEC mode.

---

## Comparing Password Protection

After configuring the enable secret, I viewed the running configuration again.

![Viewing the protected password entries](<SW1 showing MD5 protected password.png>)

*Reviewing the protected password entries in the SW1 running configuration.*

In the Cisco IOS version simulated by Packet Tracer for this lab, I observed two different password representations:

| Configuration | Type Displayed | Purpose |
|---|---:|---|
| `enable password` after `service password-encryption` | Type 7 | Obfuscates the configured password |
| `enable secret` | Type 5 | Stores the secret using an MD5-based hash in this lab environment |

This demonstrated that `enable password` and `enable secret` do not provide the same level of password protection.

The `enable secret` provides stronger protection than the legacy `enable password` mechanism used in this exercise.

---

## Cisco IOS Command Modes

This lab reinforced that Cisco IOS commands depend on the command mode I am currently using.

Some of the modes I worked with were:

### User EXEC Mode

```text
SW1>
```

This is the initial mode with limited access to device information.

### Privileged EXEC Mode

```text
SW1#
```

I entered this mode using:

```text
enable
```

Privileged EXEC mode provides access to more advanced monitoring and administrative commands.

### Global Configuration Mode

```text
SW1(config)#
```

I entered this mode using:

```text
configure terminal
```

This mode is used to make changes to the device configuration.

Understanding which mode I was currently in became especially important during one of the troubleshooting situations I encountered.

---

## Troubleshooting

### Issue 1 — `show running-config` Returned an Error

While I was in Global Configuration mode, I attempted to enter:

```text
show running-config
```

Cisco IOS returned:

```text
% Invalid input detected at '^' marker.
```

### Investigation

I realized that `show running-config` is a **Privileged EXEC command**, but my prompt showed:

```text
SW1(config)#
```

This told me that I was still in Global Configuration mode.

Instead of exiting Global Configuration mode every time I wanted to run a Privileged EXEC command, I realized I could use the `do` command.

### Resolution

I entered:

```text
do show running-config
```

This allowed the command to execute successfully while I remained in Global Configuration mode.

### What I Learned

This reinforced that Cisco IOS commands are **mode-dependent**.

If I enter a command that I know exists but receive:

```text
% Invalid input detected
```

one of the first things I should check is the command prompt to determine which IOS mode I am currently using.

I also learned that `do` is useful when I need to execute a Privileged EXEC command without leaving configuration mode.

---

## Saving the Running Configuration

The final task was to save the active running configuration to the startup configuration.

Normally, this can be done from Privileged EXEC mode using:

```text
copy running-config startup-config
```

Because I was still in Global Configuration mode, I used:

```text
do copy running-config startup-config
```

This led to the second troubleshooting issue I encountered during the lab.

---

## Troubleshooting

### Issue 2 — Misunderstanding the Destination Filename Prompt

After entering:

```text
do copy running-config startup-config
```

Cisco IOS displayed:

```text
Destination filename [startup-config]?
```

I initially did not understand what the prompt was asking me.

I interpreted it like a yes-or-no confirmation prompt, so I entered:

```text
y
```

and pressed Enter.

IOS then returned an error because it interpreted `y` as the destination filename instead of as confirmation.

### Investigation

After seeing the error, I realized that:

```text
[startup-config]
```

was already the default destination filename.

The device was not asking:

```text
Are you sure? y/n
```

Instead, it was asking me to either:

- Enter a different destination filename, or
- Press **Enter** to accept the displayed default of `startup-config`

### Resolution

I ran the command again:

```text
do copy running-config startup-config
```

When IOS displayed:

```text
Destination filename [startup-config]?
```

I pressed **Enter without typing anything**.

The configuration was then copied successfully.

![Saving the running configuration to startup configuration](<SW1 saving running to startup.png>)

*My first attempt using `y`, followed by the successful attempt where I accepted the default `startup-config` filename by pressing Enter.*

### What I Learned

This taught me to carefully read Cisco IOS prompts instead of assuming that every confirmation prompt expects `y` or `n`.

When IOS displays a value inside brackets, such as:

```text
[startup-config]
```

that value represents the default choice.

Pressing Enter accepts that default.

---

## Running Configuration vs. Startup Configuration

This lab also reinforced the difference between the running configuration and startup configuration.

### Running Configuration

The **running configuration** is the configuration currently active on the device.

It can be viewed using:

```text
show running-config
```

Configuration changes are made to the running configuration while the device is operating.

### Startup Configuration

The **startup configuration** is the saved configuration that the device can use when it boots.

To copy the current configuration into startup configuration, I used:

```text
copy running-config startup-config
```

or, while remaining in Global Configuration mode:

```text
do copy running-config startup-config
```

This helped reinforce that making a configuration change and saving a configuration are two separate actions.

If changes remain only in the running configuration, they can be lost if the device restarts before the configuration is saved.

---

## Commands Used

Some of the Cisco IOS commands I practiced during this lab included:

```text
enable
configure terminal
hostname SW1
hostname R1
enable password CCNA
service password-encryption
enable secret Cisco
show running-config
do show running-config
copy running-config startup-config
do copy running-config startup-config
exit
```

---

## Verification

I verified my work throughout the lab by:

1. Confirming that the device prompts reflected the new hostnames
2. Exiting to User EXEC mode
3. Using `enable` to return to Privileged EXEC mode
4. Confirming that the configured password was required
5. Viewing the running configuration
6. Enabling `service password-encryption`
7. Reviewing the running configuration again
8. Confirming that the `enable password` was no longer displayed as plaintext
9. Configuring the more secure `enable secret`
10. Testing Privileged EXEC access again
11. Comparing the password entries in the running configuration
12. Saving the running configuration to startup configuration

The same required configuration tasks were completed on both **SW1** and **R1**.

The screenshots in this README show SW1 as the documented example.

---

## What I Learned

This lab gave me more hands-on experience with the Cisco IOS CLI and helped connect several device configuration, security, and configuration-management concepts.

### IOS Command Mode Matters

Cisco IOS commands are available from specific command modes.

When a valid command did not work, looking at my current prompt helped me determine that I was in the wrong mode.

### `do` Can Execute EXEC Commands From Configuration Mode

Instead of repeatedly leaving Global Configuration mode to run verification commands, I learned that I can use:

```text
do show running-config
```

This allows me to perform certain EXEC-level tasks while remaining in configuration mode.

### Password Protection Methods Are Different

The lab demonstrated that:

```text
enable password
```

and:

```text
enable secret
```

do not provide the same level of protection.

I also observed how:

```text
service password-encryption
```

changes how applicable plaintext passwords appear in the configuration.

### Cisco IOS Prompts Need to Be Read Carefully

My mistake when saving the configuration taught me that Cisco IOS prompts do not always expect a yes-or-no response.

When IOS displayed:

```text
Destination filename [startup-config]?
```

the value in brackets represented the default.

Pressing Enter accepted that value.

### Configurations Need to Be Saved

Making changes to the running configuration does not automatically mean those changes are saved for the next device boot.

I need to understand the difference between:

```text
running-config
```

and:

```text
startup-config
```

and intentionally save configurations when required.

### Troubleshooting the CLI Is Part of Learning IOS

Both issues I encountered were caused by how I interacted with Cisco IOS rather than by a network connectivity problem.

In both cases, I was able to review what IOS was telling me, identify what I had done incorrectly, correct it, and continue the lab.

That gave me additional practice reading CLI feedback instead of simply treating an error message as a failed command.

---

## Screenshots

The screenshots included in this lab document my SW1 configuration process:

- `SW1 changing hostname.png`
- `SW1 enabling unencrypted password.png`
- `SW1 Password enabled successfully.png`
- `SW1 show running config file to see passwords.png`
- `SW1 current-future passwords will be encrypted.png`
- `SW1 used a more secure password encryption -MD5 secret.png`
- `SW1 showing MD5 protected password.png`
- `SW1 saving running to startup.png`

The same required configuration tasks were also completed on R1.

---

## Study Source

This lab was completed as part of my CCNA studies using **Jeremy's IT Lab**.

The documentation, explanations, screenshots, troubleshooting steps, observations, and lessons learned in this repository are written in my own words as a record of my hands-on learning.
