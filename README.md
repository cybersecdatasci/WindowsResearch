### WindowsResearch

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
