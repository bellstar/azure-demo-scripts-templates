# Groundwork for Windows Server Failover Clustering lab environment

This template provides groundworks for the Windows Server Failover Clustering lab environment.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ftksh164%2Fazure-demo-scripts-templates%2Fmaster%2Farm-templates%2Fgroundwork-wsfc%2Ftemplate.json)

## Deployment

- Domain controller VM
    - The AD DS feature and related management tools are installed.
    - The data disk for AD DS is formatted with drive letter `N:`.
- WSFC node 1 VM
    - The Failover Clustering feature and related management tools are installed.
    - The shared data disk for witness is formatted with drive letter `W:` if this VM's fault domain is equals `0`.
- WSFC node 2 VM
    - The Failover Clustering feature is installed.
    - The shared data disk for witness is formatted with drive letter `W:` if this VM's fault domain is equals `0`.
- Client VM

### Localization scripts

You can use localization scripts if you need localization. The localization scripts for Japanese are located under the `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\x.y.z\Downloads\0`.

You should run the localization scripts before server's role setup (e.g. Domain controller, Failover cluster node).

The two scripts change the operating system's language settings.

Localization steps:

1. Install the language pack and language capabilities by `lang-jajp-step1.ps1`. The system reboots after finish the script. This script takes few minutes for running.

    ```powershell
    PS > .\lang-jajp-step1.ps1
    ```

2. Change language related settings by `lang-jajp-step2.ps1`. The system reboots after finish the script. This script takes less a minute for running.

    ```powershell
    PS > .\lang-jajp-step2.ps1
    ```

### Domain controller VM

```powershell
Install-ADDSForest -DomainName lab.contoso.com -DatabasePath N:\Windows\NTDS -LogPath N:\Windows\NTDS -SysvolPath N:\Windows\SYSVOL -Force -Verbose
```

### WSFC nodes

```powershell
New-Cluster -Name clus1 -ManagementPointNetworkType Distributed -Node n1,n2
```

## Notes

- The deploy.ps1 script needs [Az module](https://www.powershellgallery.com/packages/Az/).