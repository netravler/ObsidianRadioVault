/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/team-lakitu_Persistent_Resources/providers/Microsoft.Compute/virtualMachines/OctopusNexus

{
    "name": "OctopusNexus",
    "id": "/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/team-lakitu_Persistent_Resources/providers/Microsoft.Compute/virtualMachines/OctopusNexus",
    "type": "Microsoft.Compute/virtualMachines",
    "location": "northcentralus",
    "tags": {
        "purpose": "host octopus"
    },
    "properties": {
        "vmId": "ea6a6774-6626-42c6-a23e-df880b2e06f2",
        "hardwareProfile": {
            "vmSize": "Standard_D2s_v3"
        },
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2019-Datacenter",
                "version": "latest"
            },
            "osDisk": {
                "osType": "Windows",
                "name": "OctopusNexus_disk1_070071b78e294ca6962cedc470083cd5",
                "createOption": "FromImage",
                "caching": "ReadWrite",
                "managedDisk": {
                    "storageAccountType": "Premium_LRS",
                    "id": "/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/TEAM-LAKITU_PERSISTENT_RESOURCES/providers/Microsoft.Compute/disks/OctopusNexus_disk1_070071b78e294ca6962cedc470083cd5"
                },
                "diskSizeGB": 127
            },
            "dataDisks": []
        },
        "osProfile": {
            "computerName": "OctopusNexus",
            "adminUsername": "pheintz",
            "windowsConfiguration": {
                "provisionVMAgent": true,
                "enableAutomaticUpdates": true
            },
            "secrets": []
        },
        "networkProfile": {
            "networkInterfaces": [
                {
                    "id": "/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/team-lakitu_Persistent_Resources/providers/Microsoft.Network/networkInterfaces/octopusnexus766"
                }
            ]
        },
        "diagnosticsProfile": {
            "bootDiagnostics": {
                "enabled": true
            }
        },
        "provisioningState": "Succeeded"
    },
    "resources": [
        {
            "name": "AdminCenter",
            "id": "/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/team-lakitu_Persistent_Resources/providers/Microsoft.Compute/virtualMachines/OctopusNexus/extensions/AdminCenter",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "northcentralus",
            "properties": {
                "autoUpgradeMinorVersion": true,
                "provisioningState": "Succeeded",
                "publisher": "Microsoft.AdminCenter",
                "type": "AdminCenter",
                "typeHandlerVersion": "0.0",
                "settings": {
                    "port": "6516",
                    "salt": "06347456501036000000880000550000300000",
                    "cspFrameAncestors": [
                        "https://portal.azure.com",
                        "https://*.hosting.portal.azure.net",
                        "https://localhost:1340"
                    ],
                    "corsOrigins": [
                        "https://portal.azure.com",
                        "https://waconazure.com"
                    ]
                }
            }
        },
        {
            "name": "AzureNetworkWatcherExtension",
            "id": "/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/team-lakitu_Persistent_Resources/providers/Microsoft.Compute/virtualMachines/OctopusNexus/extensions/AzureNetworkWatcherExtension",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "northcentralus",
            "properties": {
                "autoUpgradeMinorVersion": true,
                "provisioningState": "Succeeded",
                "publisher": "Microsoft.Azure.NetworkWatcher",
                "type": "NetworkWatcherAgentWindows",
                "typeHandlerVersion": "1.4",
                "settings": {}
            }
        },
        {
            "name": "Microsoft.Insights.VMDiagnosticsSettings",
            "id": "/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/team-lakitu_Persistent_Resources/providers/Microsoft.Compute/virtualMachines/OctopusNexus/extensions/Microsoft.Insights.VMDiagnosticsSettings",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "northcentralus",
            "properties": {
                "autoUpgradeMinorVersion": true,
                "provisioningState": "Succeeded",
                "publisher": "Microsoft.Azure.Diagnostics",
                "type": "IaaSDiagnostics",
                "typeHandlerVersion": "1.5",
                "settings": {
                    "StorageAccount": "bigipplacetostore",
                    "WadCfg": {
                        "DiagnosticMonitorConfiguration": {
                            "overallQuotaInMB": 5120,
                            "Metrics": {
                                "resourceId": "/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/TEAM-LAKITU_PERSISTENT_RESOURCES/providers/Microsoft.Compute/virtualMachines/OctopusNexus",
                                "MetricAggregation": [
                                    {
                                        "scheduledTransferPeriod": "PT1H"
                                    },
                                    {
                                        "scheduledTransferPeriod": "PT1M"
                                    }
                                ]
                            },
                            "DiagnosticInfrastructureLogs": {
                                "scheduledTransferLogLevelFilter": "Error",
                                "scheduledTransferPeriod": "PT1M"
                            },
                            "PerformanceCounters": {
                                "scheduledTransferPeriod": "PT1M",
                                "PerformanceCounterConfiguration": [
                                    {
                                        "counterSpecifier": "\\Processor Information(_Total)\\% Processor Time",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Processor Information(_Total)\\% Privileged Time",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Processor Information(_Total)\\% User Time",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Processor Information(_Total)\\Processor Frequency",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\System\\Processes",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(_Total)\\Thread Count",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(_Total)\\Handle Count",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\System\\System Up Time",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\System\\Context Switches/sec",
                                        "unit": "CountPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\System\\Processor Queue Length",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Memory\\% Committed Bytes In Use",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Memory\\Available Bytes",
                                        "unit": "Bytes",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Memory\\Committed Bytes",
                                        "unit": "Bytes",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Memory\\Cache Bytes",
                                        "unit": "Bytes",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Memory\\Pool Paged Bytes",
                                        "unit": "Bytes",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Memory\\Pool Nonpaged Bytes",
                                        "unit": "Bytes",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Memory\\Pages/sec",
                                        "unit": "CountPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Memory\\Page Faults/sec",
                                        "unit": "CountPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(_Total)\\Working Set",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(_Total)\\Working Set - Private",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\% Disk Time",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\% Disk Read Time",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\% Disk Write Time",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\% Idle Time",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Disk Bytes/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Disk Read Bytes/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Disk Write Bytes/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Disk Transfers/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Disk Reads/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Disk Writes/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Avg. Disk sec/Transfer",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Avg. Disk sec/Read",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Avg. Disk sec/Write",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Avg. Disk Queue Length",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Avg. Disk Read Queue Length",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Avg. Disk Write Queue Length",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\% Free Space",
                                        "unit": "Percent",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\LogicalDisk(_Total)\\Free Megabytes",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Network Interface(*)\\Bytes Total/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Network Interface(*)\\Bytes Sent/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Network Interface(*)\\Bytes Received/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Network Interface(*)\\Packets/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Network Interface(*)\\Packets Sent/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Network Interface(*)\\Packets Received/sec",
                                        "unit": "BytesPerSecond",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Network Interface(*)\\Packets Outbound Errors",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Network Interface(*)\\Packets Received Errors",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Exceptions(w3wp)\\# of Exceps Thrown / sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Interop(w3wp)\\# of marshalling",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Jit(w3wp)\\% Time in Jit",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Loading(w3wp)\\Current appdomains",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Loading(w3wp)\\Current Assemblies",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Loading(w3wp)\\% Time Loading",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Loading(w3wp)\\Bytes in Loader Heap",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR LocksAndThreads(w3wp)\\Contention Rate / sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR LocksAndThreads(w3wp)\\Current Queue Length",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Memory(w3wp)\\# Gen 0 Collections",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Memory(w3wp)\\# Gen 1 Collections",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Memory(w3wp)\\# Gen 2 Collections",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Memory(w3wp)\\% Time in GC",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Memory(w3wp)\\# Bytes in all Heaps",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Networking(w3wp)\\Connections Established",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Networking 4.0.0.0(w3wp)\\Connections Established",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\.NET CLR Remoting(w3wp)\\Remote Calls/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Application Restarts",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Applications Running",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Requests Current",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Request Execution Time",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Request Wait Time",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Requests Disconnected",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Worker Processes Running",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET\\Worker Process Restarts",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Application Restarts",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Applications Running",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Requests Current",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Request Execution Time",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Requests Queued",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Requests Rejected",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Request Wait Time",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Requests Disconnected",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Worker Processes Running",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET v4.0.30319\\Worker Process Restarts",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Anonymous Requests",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Anonymous Requests/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache Total Entries",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache Total Turnover Rate",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache Total Hits",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache Total Misses",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache Total Hit Ratio",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache API Entries",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache API Turnover Rate",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache API Hits",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache API Misses",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Cache API Hit Ratio",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Output Cache Entries",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Output Cache Turnover Rate",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Output Cache Hits",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Output Cache Misses",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Output Cache Hit Ratio",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Compilations Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Debugging Requests",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors During Preprocessing",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors During Compilation",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors During Execution",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Unhandled During Execution",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Unhandled During Execution/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Pipeline Instance Count",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Request Bytes In Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Request Bytes Out Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests Executing",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests Failed",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests Not Found",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests Not Authorized",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests In Application Queue",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests Timed Out",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests Succeeded",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Sessions Active",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Sessions Abandoned",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Sessions Timed Out",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Sessions Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Transactions Aborted",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Transactions Committed",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Transactions Pending",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Transactions Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Transactions/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Anonymous Requests",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Anonymous Requests/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache Total Entries",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache Total Turnover Rate",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache Total Hits",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache Total Misses",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache Total Hit Ratio",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache API Entries",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache API Turnover Rate",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache API Hits",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache API Misses",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Cache API Hit Ratio",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Output Cache Entries",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Output Cache Turnover Rate",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Output Cache Hits",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Output Cache Misses",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Output Cache Hit Ratio",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Compilations Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Debugging Requests",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Errors During Preprocessing",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Errors During Compilation",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Errors During Execution",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Errors Unhandled During Execution",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Errors Unhandled During Execution/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Errors Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Errors Total/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Pipeline Instance Count",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Request Bytes In Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Request Bytes Out Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests Executing",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests Failed",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests Not Found",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests Not Authorized",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests In Application Queue",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests Timed Out",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests Succeeded",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Requests/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Sessions Active",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Sessions Abandoned",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Sessions Timed Out",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Sessions Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Transactions Aborted",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Transactions Committed",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Transactions Pending",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Transactions Total",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\ASP.NET Apps v4.0.30319(__Total__)\\Transactions/Sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(w3wp)\\% Processor Time",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(w3wp)\\Virtual Bytes",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(w3wp)\\Private Bytes",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(w3wp)\\Thread Count",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Process(w3wp)\\Handle Count",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Web Service(_Total)\\Current Connections",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Web Service(_Total)\\Total Method Requests/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:Buffer Manager\\Page reads/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:Buffer Manager\\Page writes/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:Buffer Manager\\Checkpoint pages/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:Buffer Manager\\Lazy writes/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:Buffer Manager\\Buffer cache hit ratio",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:Buffer Manager\\Database pages",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:Memory Manager\\Total Server Memory (KB)",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:Memory Manager\\Memory Grants Pending",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:General Statistics\\User Connections",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:SQL Statistics\\Batch Requests/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:SQL Statistics\\SQL Compilations/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    },
                                    {
                                        "counterSpecifier": "\\SQLServer:SQL Statistics\\SQL Re-Compilations/sec",
                                        "unit": "Count",
                                        "sampleRate": "PT60S"
                                    }
                                ]
                            },
                            "WindowsEventLog": {
                                "scheduledTransferPeriod": "PT1M",
                                "DataSource": [
                                    {
                                        "name": "Application!*[System[(Level=1 or Level=2 or Level=3)]]"
                                    },
                                    {
                                        "name": "System!*[System[(Level=1 or Level=2 or Level=3)]]"
                                    },
                                    {
                                        "name": "Security!*[System[(band(Keywords,4503599627370496))]]"
                                    }
                                ]
                            },
                            "Directories": {
                                "scheduledTransferPeriod": "PT1M"
                            }
                        }
                    }
                }
            }
        },
        {
            "name": "SqlIaasExtension",
            "id": "/subscriptions/7440f141-afc3-496b-b1fd-0b007c1261e4/resourceGroups/team-lakitu_Persistent_Resources/providers/Microsoft.Compute/virtualMachines/OctopusNexus/extensions/SqlIaasExtension",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "northcentralus",
            "properties": {
                "autoUpgradeMinorVersion": true,
                "provisioningState": "Succeeded",
                "publisher": "Microsoft.SqlServer.Management",
                "type": "SqlIaaSAgent",
                "typeHandlerVersion": "2.0",
                "settings": {
                    "SqlManagement": {
                        "IsEnabled": false
                    },
                    "DeploymentTokenSettings": {
                        "DeploymentToken": 147336752
                    }
                }
            }
        }
    ]
}