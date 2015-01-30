# Chocolatey Push (choco push / cpush)
Chocolatey will attempt to push a compiled nupkg to a package feed.
 That feed can be a local folder, a file share, the community feed
 'https://chocolatey.org/' or a custom/private feed.

## Usage

    choco push [path to nupkg] [options/switches]

**NOTE**: If there is more than one nupkg file in the folder, the command
 will require specifying the path to the file.

## Examples

    choco push --source "https://chocolatey.org/"
    choco push --source "https://chocolatey.org/" -t 500
    choco push --source "https://chocolatey.org/" -k="123-123123-123"

## Options and Switches

Includes [[default options/switches|CommandsReference#default-options-and-switches]]

```
-s, --source=VALUE
  Source - The source we are pushing the package to. Use https://chocolatey.org/
  to push to community feed.

-k, --key, --apikey, --api-key=VALUE
  ApiKey - The api key for the source. If not specified (and not local
  file source), does a lookup. If not specified and one is not found for
  an https source, push will fail.

-t, --timeout=VALUE
  Timeout (in seconds) - The time to allow a package push to occur
  before timing out. Defaults to 300 seconds (5 minutes).
```

### Common Troubleshooting

**NOTE** : To use this command, you must have your API key saved for
chocolatey.org or the source you want to push to. Or you can pass the apikey to
the command.
In order to save your API key, copy the access key from your chocolatey.org account into the following command:
`choco apikey <your key here> -source https://chocolatey.org/`

`Failed to process request. 'The specified API key does not provide the authority to push packages.'
  The remote server returned an error: (403) Forbidden..`
This means the package already exists with a different user (API key).  The package could be unlisted. Please contact one of the administrators of chocolatey.org if you see this and you don't see a good reason for it.
