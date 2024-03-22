# RHEL/CentOS 7 Goss config

## Overview

based on CIS 4.0.0

Set of configuration files and directories to run the first stages of CIS of RHEL/CentOS 7 servers

This is configured in a directory structure level.

This could do with further testing but sections 1.x should be complete

Goss is run based on the goss.yml file in the top level directory. This specifies the configuration.

## Join us

On our [Discord Server](https://www.lockdownenterprise.com/discord) to ask questions, discuss features, or just chat with other Ansible-Lockdown users

## Requirements

You must have [goss](https://goss.rocks) available to your host you would like to test.

You must have sudo/root access to the system as some commands require privilege information.

Assuming you have already clone this repository you can run goss from where you wish.

Please refer to the audit documentation for usage.

This also works alongside the [Ansible Lockdown AMAZON2-CIS role](https://github.com/ansible-lockdown/AMAZON2-CIS)

Which will:

- install
- audit
- remediate
- audit

## variables

these are found in vars/cis.yml
Please refer to the file for all options and their meanings

CIS listed variable for every control/benchmark can be turned on/off or section

- other controls
enable_selinux
run_heavy_tasks

- bespoke options
If a site has specific options e.g. password complexity these can also be set.

## Usage

You must have [goss](https://goss.rocks) available to your host you would like to test.

You must have root access to the system as some commands require privilege information.

- Run as root not sudo due to sudo and shared memory access

Assuming you have already clone this repository you can run goss from where you wish.

- full check

```sh
# {{path to your goss binary}} --vars {{ path to the vars file }} -g {{path to your clone of this repo }}/goss.yml --validate

```

example:

```sh
# /usr/local/bin/goss --vars ../vars/cis.yml -g /home/bolly/rh7_cis_goss/goss.yml validate
......FF....FF................FF...F..FF.............F........................FSSSS.............FS.F.F.F.F.........FFFFF....

Failures/Skipped:

Title: 1.6.1 Ensure core dumps are restricted (Automated)_sysctl
Command: suid_dumpable_2: exit-status:
Expected
    <int>: 1
to equal
    <int>: 0
Command: suid_dumpable_2: stdout: patterns not found: [fs.suid_dumpable = 0]


Title: 1.4.2 Ensure filesystem integrity is regularly checked (Automated)
Service: aidecheck: enabled:
Expected
    <bool>: false
to equal
    <bool>: true
Service: aidecheck: running:
Expected
    <bool>: false
to equal
    <bool>: true

< ---------cut ------- >

Title: 1.1.22 Ensure sticky bit is set on all world-writable directories
Command: version: exit-status:
Expected
    <int>: 0
to equal
    <int>: 123

Total Duration: 5.102s
Count: 124, Failed: 21, Skipped: 5

```

- running a particular section of tests

```sh
# /usr/local/bin/goss -g /home/bolly/rh7_cis_goss/section_1/cis_1.1/cis_1.1.22.yml  validate
............

Total Duration: 0.033s
Count: 12, Failed: 0, Skipped: 0

```

- changing the output

```sh
# /usr/local/bin/goss -g /home/bolly/rh7_cis_goss/section_1/cis_1.1/cis_1.4.1.yml  validate -f documentation
Title: 1.4.1 | Ensure address space layout randomization (ASLR) is enabled | sysctl_configured
Meta:
    CIS_ID: [1.4.1]
    CISv8: [10.5]
    CISv8_IG1: true
    CISv8_IG2: true
    CISv8_IG3: true
    NIST800-53R5: [CM-6 CM-6b]
    server: 1
    workstation: NA
Command: aslr_enabled_2: stdout:
Expected
    "object: *bytes.Reader"
to have patterns
    ["/^kernel.randomize_va_space=2/"]
the missing elements were
    ["/^kernel.randomize_va_space=2/"]


Total Duration: 0.022s
Count: 12, Failed: 0, Skipped: 0
```

## Variables

### The variable files

sections_2/service.yml allows you to tune it further for specific environments.

In this case installed or skipped using the standard name for a package to be installed or _skip to skip a test.

## Extra settings

## further information

- [goss documentation](https://github.com/aelsabbahy/goss/blob/master/docs/manual.md#patterns)
- [CIS standards](https://www.cisecurity.org)
