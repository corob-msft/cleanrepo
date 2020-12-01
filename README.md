# CleanRepo

This command-line tool helps you clean up a DocFx-based content repo. It can:

- find and delete markdown files that aren't linked from a TOC file
- find and delete orphaned image (.png, .jpg, .gif, .svg) files
- find and delete orphaned snippet (.cs, .vb) files
- find and delete orphaned "shared" markdown files
- trim unused redirect entries from an .openpublishing.redirection.json file
- remove "hops" in redirect entries
- find and replace links to redirected files
- replace site-relative links with file-relative links (includes image links)
- find articles that appear more than once in a TOC file

## Usage

| Command | Description |
| - | - |
| -d, --directory | Top-level directory in which to perform clean up (for example, find orphaned markdown files). |
| --orphaned-topics | Use this option to find orphaned topics. |
| --multiples | Use this option to find topics that appear more than once in one or separate TOC.md files. |
| --orphaned-images | Find orphaned .png, .gif, .svg, or .jpg files. |
| --orphaned-snippets | Find orphaned .cs and .vb files. |
| --orphaned-includes | Find orphaned INCLUDE files. |
| -g, --delete | (Default: False) Delete orphaned markdown or .png/.jpg/.gif files. |
| --remove-hops | Replace redirect targets that are themselves redirected. |
| --replace-redirects | Find backlinks to redirected files and replace with new target. |
| --format-redirects | Format the redirection JSON file by deserializing and then serializing with pretty printing. （Recommend doing this before --trim-redirects to make the trimming diff useful.) |
| --trim-redirects | Remove redirect entries for pages that haven't been viewed in the specified number of days. |
| --lookback-days | (Default: 90) The number of days to check for page-view activity. |
| --redirects-file | Optionally specify a path to a redirect JSON file in a different repo. |
| --relative-links | Replace site-relative links with file-relative links.  You must also specify the docset name for the repo. |
| --docset-name | The docset name that corresponds to the root of this repo in a URL, e.g. 'visualstudio' in 'http://docs.microsoft.com/visualstudio/ide/get-started'. |
| --docset-root | The full path to the root directory for the docset, e.g. 'c:\users\gewarren\dotnet-docs\docs'. |
| -s, --recursive | (Default: True) Search directory and all subdirectories for markdown, yaml, image, and include files (depending on chosen function). |

## Usage examples

Find orphaned articles recursively (that is, in the specified directory and any subdirectories):

```
CleanRepo.exe --orphaned-topics -d c:\repos\visualstudio-docs-pr\docs\ide
```

Find orphaned topics non-recursively (that is, only in the specified directory):

```
CleanRepo.exe --orphaned-topics -s false -d c:\repos\visualstudio-docs-pr\docs\ide
```

Find and delete orphaned .png/.gif/.jpg/.svg files (recursive):

```
CleanRepo.exe --orphaned-images -g -d c:\repos\visualstudio-docs-pr\docs\ide --docset-name visualstudio
```

Find and delete orphaned .cs/.vb files (recursive):

```
CleanRepo.exe --orphaned-snippets -g -d c:\repos\visualstudio-docs-pr\docs\ide\snippets
```

Find and delete shared markdown files that are orphaned (recursive):

```
CleanRepo.exe --orphaned-includes -g -d c:\repos\visualstudio-docs-pr\docs\ide
```

Format the redirection JSON file with pretty printing serialization.

```bash
CleanRepo.exe --format-redirects --docset-root c:\users\gewarren\dotnet-docs\docs
```

Remove entries in the .openpublishing.redirection.json file that haven't been clicked in the last 180 days:

```
CleanRepo.exe --trim-redirects --docset-name dotnet --docset-root c:\users\gewarren\dotnet-docs\docs --lookback-days 180
```

Remove bunny hops in the .openpublishing.redirection.json file by replacing any redirect URLs with their respective redirect URL, if it exists:

```
CleanRepo.exe --remove-hops --docset-name visualstudio --docset-root c:\repos\visualstudio-docs-pr\docs
```

Find articles with backlinks to redirected topics, and replace the links with their target URL:

```
CleanRepo.exe --replace-redirects -d c:\repos\visualstudio-docs-pr\docs\ide
```

> [!TIP]
> Some redirect targets are themselves redirected to yet another target. For this reason, it's recommended to run the --remove-hops option before the --replace-redirects option.

Replace site-relative links to the specified docset with file-relative links, when the file exists (includes image links):

```
CleanRepo.exe --relative-links --docset-name visualstudio --docset-root c:\repos\visualstudio-docs-pr\docs -d c:\repos\visualstudio-docs-pr\docs\ide
```

Search recursively for articles that appear more than once in a TOC file:

```
CleanRepo.exe -m -d c:\repos\visualstudio-docs-pr\docs\ide
```

## Future functionality ideas...

- Find orphaned code snippets
