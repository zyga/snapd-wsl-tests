<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: Canonical Ltd.
-->

# Snapd in WSL

Smoke testing snapd in WSL-2!

This repository contains GitHub workflow designed to run on Windows
to install and prepare a WSL-2 environment and go through several
scenarios using _snap_ packages.

At present there are three test scenarios available:

- A `hello` snap is installed and invoked.
- A `docker` snap is installed and used to spawn a command in an OCI container.
- A `snapcraft` snap is installed alongside `lxd` snap, then both are used to
  build a snap from source.

In all cases the snaps represent various grades of complexity and interaction
with the Windows interoperability layer. In all cases `snapd` snap is installed
first. Mechanism exist to allow selecting either a specific _revision_ or a
specific _channel_ where snapd is installed from. This is designed such so that
the workflow may be used to test upcoming releases of snapd.

The `hello` snap is the most basic test - it should work in all the cases as it
has minimal interaction surface with the rest of the system. The `docker` snap
is the extreme opposite of that, as it uses many complex elements of the stack.
Lastly `lxd` and `snapcraft` used together to build an application snap
represents a workload that developers may try to achieve using WSL.

# GitHub Runner

This section describes the technical needs to run this workflow either in the
public GitHub runners or with a self-hosted runner.

## GitHub Runner (standard)

There's nothing you have to do. The workflow work out-of-the box on
`windows-latest` runners.

## GitHub Runner (self-hosted)

If you are unfamiliar with Windows, the following set of instructions might help:

- Install Windows 11
  - Use a local account for best results.
  - The user should be able to administer the system (belong to the _Administrators_ group).
  - Update the system, install virtualization tools if not using bare metal.
- (Optional) Install openSSH server for easier administration.
  - It may be installed from application list, optional features
- (Optional) Set default shell used by openSSH to powershell by running
```psh
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```
- Set script execution to _bypass_ by running the following PowerShell command
  as the same user who will later on run the github-runner service.
```psh
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope CurrentUser
```
- (Optional) Snapshot the system using your virtual machine management software.
- Set up github-runner on Windows according to standard installation
  instructions for self-hosted runners offered by GitHub with the following exceptions:
  - Use the same user account you've created when installing Windows
  - Run github-runner as a service
