# cDnsServer

The **cDnsServer** DSC resources configure and manage a DNS server. They include **cDnsServerSecondaryZone**, **cDnsServerZoneTransfer** and **cDnsRecord**.

## Contributing
Please check out common DSC Resources [contributing guidelines](https://github.com/PowerShell/DscResource.Kit/blob/master/CONTRIBUTING.md).


## Resources

* **cDnsServerSecondaryZone** sets a Secondary zone on a given DNS server.
Secondary zones allow client machine in primary DNS zones to do DNS resolution of machines in the secondary DNS zone.
* **cDnsServerZoneTransfer** This resource allows a DNS Server zone data to be replicated to another DNS server.
* **cDnsRecord** This resource allwos for the creation of A records agaisnt a specific zone on the DNS server


### cDnsServerSecondaryZone

* **Name**: Name of the secondary zone
* **MasterServers**: IP address or DNS name of the secondary DNS servers
* **Ensure**: Whether the secondary zone should be present or removed
* **Type**: Type of the DNS server zone

### cDnsServerZoneTransfer

* **Name**: Name of the DNS zone
* **Type**: Type of transfer allowed. 
Values include: { None | Any | Named | Specific }
* **SecondaryServer**: IP address or DNS name of DNS servers where zone information can be transfered.

### cDNSRecord
* **Name**: Name of the DNS zone
* **Zone**: The name of the zone to create the record in
* **Target**: The IP address of the A record
* **Type**: DNS Record Type.
Values include: { A-Record | C-Name }


## Versions

### 1.3.2.0

* Renamed DNSArecord to DNSRecord and added C-Names functionality

### 1.3.0.0

* Fix to retrieving settings for record data

### 1.2.0.0

* Removed UTF8 BOM from MOF schema

### 1.1

* Add **cDnsRecord** resource.

### 1.0

*   Initial release with the following resources 
    * **cDnsServerSecondaryZone**
    * **cDnsServerZoneTransfer**

## Examples

### Configuring a DNS Transfer Zone

```powershell
configuration Sample_cDnsServerZoneTransfer_TransferToAnyServer
{
    param
    (
        [Parameter(Mandatory)]
        [String]$DnsZoneName,

        [Parameter(Mandatory)]
        [String]$TransferType
    )
    Import-DscResource -module cDnsServer
    cDnsServerZoneTransfer TransferToAnyServer
    {
        Name = $DnsZoneName
        Type = $TransferType
    }
}
Sample_cDnsServerZoneTransfer_TransferToAnyServer -DnsZoneName 'demo.contoso.com' -TransferType 'Any'
```

### Configuring a Secondary DNS Zone

```powershell
configuration Sample_cDnsServerSecondaryZone
{
    param
    (
        [Parameter(Mandatory)]
        [String]$ZoneName,
        [Parameter(Mandatory)]
        [String[]]$SecondaryDnsServer
    )

    Import-DscResource -module cDnsServer
    cDnsServerSecondaryZone sec
    {
        Ensure        = 'Present'                
        Name          = $ZoneName
        MasterServers = $SecondaryDnsServer

    }
}
Sample_cDnsServerSecondaryZone -ZoneName 'demo.contoso.com' -SecondaryDnsServer '192.168.10.2' 
```

### Adding a DNS A Record

```powershell
configuration Sample_Arecord
{
    Import-DscResource -module cDnsServer
    cDNSRecord TestRecord
    {
        Name = "testArecord"
        Target = "192.168.0.123"
        Zone = "contoso.com"
        Type = "A-record"
    }
}
Sample_Arecord 
```

### Adding a DNS C Name

```powershell
configuration Sample_Cname
{
    Import-DscResource -module cDnsServer
    cDNSRecord TestRecord
    {
        Name = "testCname"
        Target = "testtarget"
        Zone = "contoso.com"
        Type = "Cname"
    }
}
Sample_Cname
```

