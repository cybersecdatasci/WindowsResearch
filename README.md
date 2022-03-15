### WindowsResearch

#### `windows_events_by_provider.json`
The uniqueness of this file is that is converts the Windows Native format (the one with all the \t and \n for formatting) and puts it into a nice clean json doc wherein a few operations were conducted to make this format:
- the XML schema was munged for just k-v pairs and data types
- the field names are sorted and attributed to the description field where there is normally a placeholder such as `%1` , `%2`, etc.

The purpose is to be able to easily search and understand the various log providers, their associated event data, and what fields of value are in those events. Note: a PowerShell script pulled the original data from a 2019 server, so some log channels may be slightly different depending on your Windows OS.

This file is a json file comprised of a list of dictionaries wherein each item is a log provider. The format looks like this:

```
[
  {
    provider_name: <providername>,
    events: [
    .
    .
    ]
  },
  {
    provider_name: <providername>,
    events: [
    .
    .
    ]
   }
etc.
```
Here is a sample event within the `events` array:
```
{
        "event_id": "4624",
        "version": "2",
        "fields": {
          "SubjectUserSid": "string",
          "SubjectUserName": "string",
          "SubjectDomainName": "string",
          "SubjectLogonId": "HexInt64",
          "TargetUserSid": "string",
          "TargetUserName": "string",
          "TargetDomainName": "string",
          "TargetLogonId": "HexInt64",
          "LogonType": "unsignedInt",
          "LogonProcessName": "string",
          "AuthenticationPackageName": "string",
          "WorkstationName": "string",
          "LogonGuid": "GUID",
          "TransmittedServices": "string",
          "LmPackageName": "string",
          "KeyLength": "unsignedInt",
          "ProcessId": "HexInt64",
          "ProcessName": "string",
          "IpAddress": "string",
          "IpPort": "string",
          "ImpersonationLevel": "string",
          "RestrictedAdminMode": "string",
          "TargetOutboundUserName": "string",
          "TargetOutboundDomainName": "string",
          "VirtualAccount": "string",
          "TargetLinkedLogonId": "HexInt64",
          "ElevatedToken": "string"
        },
        "description": "An account was successfully logged on. Subject: Security ID: <SubjectUserSid> Account Name: <SubjectUserName> Account Domain: <SubjectDomainName> Logon ID: <SubjectLogonId> Logon Information: Logon Type: <LogonType> Restricted Admin Mode: <RestrictedAdminMode> Virtual Account: <VirtualAccount> Elevated Token: <ElevatedToken> Impersonation Level: <ImpersonationLevel> New Logon: Security ID: <TargetUserSid> Account Name: <TargetUserName> Account Domain: <TargetDomainName> Logon ID: <TargetLogonId> Linked Logon ID: <TargetLinkedLogonId> Network Account Name: <TargetOutboundUserName> Network Account Domain: <TargetOutboundDomainName> Logon GUID: <LogonGuid> Process Information: Process ID: <ProcessId> Process Name: <ProcessName> Network Information: Workstation Name: <WorkstationName> Source Network Address: <IpAddress> Source Port: <IpPort> Detailed Authentication Information: Logon Process: <LogonProcessName> Authentication Package: <AuthenticationPackageName> Transited Services: <TransmittedServices> Package Name (NTLM only): <LmPackageName> Key Length: <KeyLength> This event is generated when a logon session is created. It is generated on the computer that was accessed. The subject fields indicate the account on the local system which requested the logon. This is most commonly a service such as the Server service, or a local process such as Winlogon.exe or Services.exe. The logon type field indicates the kind of logon that occurred. The most common types are 2 (interactive) and 3 (network). The New Logon fields indicate the account for whom the new logon was created, i.e. the account that was logged on. The network fields indicate where a remote logon request originated. Workstation name is not always available and may be left blank in some cases. The impersonation level field indicates the extent to which a process in the logon session can impersonate. The authentication information fields provide detailed information about this specific logon request. - Logon GUID is a unique identifier that can be used to correlate this event with a KDC event. - Transited services indicate which intermediate services have participated in this logon request. - Package name indicates which sub-protocol was used among the NTLM protocols. - Key length indicates the length of the generated session key. This will be 0 if no session key was requested."
      }
```

There are a couple ways to use the data. Most simply, CTRL+F works great in a text editor. If you wanted to use the file programmatically just something quick and easy like this gets one going:

```
from json import load

filepath = '<some_filepath>'

with open(filepath) as f:
    win_events = load(f)

    for channel in win_events:

        # if you are looking for some specific channel/provider
        if channel['provider_name'] == 'Microsoft-Windows-Security-Auditing':

            for event in channel['events']:
            
                #do something....
```
