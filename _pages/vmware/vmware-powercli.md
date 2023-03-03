---
layout: post
title: VMWare PowerCLI
categories: [ VMWare ]
tags: [ vmware, powercli]
show-avatar: false
permalink: vmware-powercli
featured: false
hidden: false
# titleshort: Red Hat KB
---

```powershell
# Take snapshot
PS C:\> New-Snapshot -VM Test-1 -Name 'This is a test snapshot' -Description 'Testing this out' -Quiesce -Memory

# Get latest snapshot 
PS C:\> $snapshot1 = Get-Snapshot -VM $vm | Sort-Object -Property Created -Descending | Select -First 1

# Remove a snapshot
PS c:\> Remove-Snapshot -Snapshot $snapshot1 -RemoveChildren
```