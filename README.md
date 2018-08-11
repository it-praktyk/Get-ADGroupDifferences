# Get-ADGroupsDifference

## SYNOPSIS

PowerShell function intended to compare group membership for two Active Directory users.

## DESCRIPTION

Using this function you can compare groups membership for two users Active Directory users.
The first is reference user, the second is compared with it and as the result groups different for both users will be displayed.

## PARAMETERS

### ReferenceUser

Active Directory user object used as source for comparison - reference user

The acceptable values for this parameter are:
-- A Distinguished Name
-- A GUID (objectGUID)
-- A Security Identifier (objectSid)
-- A SAM Account Name (sAMAccountName)

### User

Active Directory user object for which group membership comparison will be performed

The acceptable values for this parameter are:
-- A Distinguished Name
-- A GUID (objectGUID)
-- A Security Identifier (objectSid)
-- A SAM Account Name (sAMAccountName)

### DomainName

Active Directory domain name - NETBIOS or FQDN - if not given than current domain for logged user is used.

### IncludeEqual

If used set to TRUE than also groups for which both users belong will be displayed. Default value is $False

## EXAMPLES

### EXAMPLE 1

```powershell
Get-ADGroupsDifference -ReferenceUser XXXX -User YYYY

    ReferenceUser          : XXXX
    User                   : YYYY
    Group                  : GroupA
    SideIndicator          : 1
    SideIndicatorName      : Only User

    ReferenceUser          : XXXX
    User                   : YYYY
    Group                  : GroupB
    SideIndicator          : -1
    SideIndicatorName      : Only ReferenceUser

    ReferenceUser          : XXXX
    User                   : YYYY
    Group                  : Group-007-License
    SideIndicator          : -1
    SideIndicatorName      : Only ReferenceUser

```

### EXAMPLE 2

```powershell
Get-ADGroupsDifference -ReferenceUser XXXX -User YYYY | Where { $_.SideIndicator -eq -1 } | ForEach { Add-ADGroupMember -Identity $_.GroupDistinguishedName -Members $_.User }
```

As the result for this command the user YYYY will be a member for all groups for the user XXXX belongs

### BASE REPOSITORY

https://github.com/it-praktyk/Get-ADGroupsDifference

## NOTES

AUTHOR: Wojciech Sciesinski, wojciech[at]sciesinski[dot]net
KEYWORDS: PowerShell, Active Directory, Groups

## LICENSE

Copyright (c) 2015-2018 Wojciech Sciesinski

This function is licensed under The MIT License (MIT)

The full license text: http://opensource.org/licenses/MIT

## VERSIONS HISTORY

- 0.3.0 - 2015-08-01 - The first version published on GitHub
- 0.3.1 - 2015-08-01 - Help updated
- 0.4.0 - 2016-08-22 - Scenarios when evaluated accounts are not members of any group added partially, the function renamed from Get-ADGroupDifferences to Get-AdGroupsDifference
- 0.4.1 - 2016-08-24 - Scenarios when evaluated accounts are not members of any group added partially, TODO added, help updated
- 0.4.2 - 2016-09-07 - Error with returning groups corrected
- 0.4.3 - 2018-01-05 - Trailing spaces removed
- 0.4.4 - 2018-08-09 - Mispelled variable name corrected - credits Ward Durossette
- 0.4.5 - 2018-08-10 - Add GroupName property and hide CN/DN paths by default to improve table formatting
                       - credits Sebastian N. @megamorf
- 0.4.6 - 2018-08-11 - Help aligned to the v. 0.4.5
