# Lab 04 — Basic Device Security

## Overview

In this lab, I practiced basic **Cisco IOS device configuration and security** on R1 and SW1.

I configured hostnames, secured Privileged EXEC access, reviewed password protection in the running configuration, and saved the completed configuration.

The screenshots below show my work on **SW1**, but I completed the same required configuration on R1.

## What I Did

### 1. Configured Device Hostnames

I changed the default hostnames to match the topology.

```text
enable
configure terminal
hostname SW1
```

The same process was completed on R1 using:

```text
hostname R1
```

![Changing the hostname on SW1](<SW1 changing hostname.png>)

### 2. Configured and Tested an Enable Password

I configured an `enable password` to protect access to Privileged EXEC mode.

```text
enable password CCNA
```

![Configuring the enable password](<SW1 enabling unencrypted password.png>)

I then exited to User EXEC mode and used:

```text
enable
```

to confirm that the device required the configured password.

![Testing the enable password](<SW1 Password enabled successfully.png>)

> **Note:** The passwords used in this lab are lab-only credentials.

### 3. Reviewed the Running Configuration

I used:

```text
show running-config
```

to verify the active device configuration and see how the password was stored.

![Viewing the running configuration](<SW1 show running config file to see passwords.png>)

### 4. Enabled Password Encryption

I enabled:

```text
service password-encryption
```

to prevent applicable passwords from appearing as readable plaintext in the configuration.

![Enabling password encryption](<SW1 current-future passwords will be encrypted.png>)

In this Packet Tracer lab, the existing `enable password` was then displayed as a **Type 7** value.

### 5. Configured an Enable Secret

I configured a more secure `enable secret`:

```text
enable secret Cisco
```

![Configuring the enable secret](<SW1 used a more secure password encryption -MD5 secret.png>)

I reviewed the running configuration again and observed that Packet Tracer displayed the `enable secret` as a **Type 5** value in this lab environment.

![Reviewing protected password entries](<SW1 showing MD5 protected password.png>)

This showed me that `enable password` and `enable secret` do not provide the same level of password protection.

### 6. Saved the Configuration

After completing the configuration, I saved the running configuration to startup configuration.

```text
copy running-config startup-config
```

Because I was working from Global Configuration mode, I also practiced using:

```text
do copy running-config startup-config
```

![Saving running configuration to startup configuration](<SW1 saving running to startup.png>)

## Troubleshooting

### Issue 1 — `show running-config` Returned an Error

**Problem:**  
I entered:

```text
show running-config
```

while I was in Global Configuration mode and received an invalid-input error.

**Cause:**  
`show running-config` is a Privileged EXEC command, but my prompt showed:

```text
SW1(config)#
```

**Fix:**  
Instead of leaving configuration mode, I used:

```text
do show running-config
```

and the command worked.

**What I learned:**  
Cisco IOS commands are mode-dependent. Checking the command prompt can help me identify when I am trying to use a command from the wrong mode.

### Issue 2 — Misread the Destination Filename Prompt

**Problem:**  
When I entered:

```text
do copy running-config startup-config
```

IOS displayed:

```text
Destination filename [startup-config]?
```

I initially entered:

```text
y
```

because I thought it was asking for yes-or-no confirmation.

IOS instead treated `y` as the destination filename and returned an error.

**Fix:**  
I ran the command again and pressed **Enter** to accept the default value shown in brackets:

```text
[startup-config]
```

![Saving running configuration to startup configuration](<SW1 saving running to startup.png>)

**What I learned:**  
A value displayed inside brackets in an IOS prompt can represent the default choice. In this case, pressing Enter accepted `startup-config`.

## Key Commands

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
```

## What I Learned

- Cisco IOS commands depend on the command mode I am currently in.
- `do` allows me to run certain EXEC commands without leaving configuration mode.
- `enable password` and `enable secret` provide different levels of password protection.
- `service password-encryption` changes how applicable plaintext passwords appear in the configuration.
- The running configuration contains the active configuration, while the startup configuration stores the saved configuration used after a restart.
- Reading IOS prompts carefully can help identify and correct CLI mistakes.

## Study Source

Completed as part of my CCNA studies using **Jeremy's IT Lab**.

This is **Lab 04** in my GitHub CCNA lab portfolio.
