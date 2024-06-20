# Snapd in WSL

Smoke testing snapd in WSL-2

# GitHub Runner

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
