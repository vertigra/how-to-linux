# Как распаковать .rar
*OC: Debian 8 Jessie*

Сначала ставим **unrar**
```bash
# apt-get install unrar
```

Что бы распаковать **file.rar**
```bash
# unrar x file.rar
```

Список опций (```unrar help```)
```bash
# unrar help

UNRAR 5.21 freeware      Copyright (c) 1993-2015 Alexander Roshal

Usage:     unrar <command> -<switch 1> -<switch N> <archive> <files...>
               <@listfiles...> <path_to_extract\>

<Commands>
  e             Extract files without archived paths
  l[t[a],b]     List archive contents [technical[all], bare]
  p             Print file to stdout
  t             Test archive files
  v[t[a],b]     Verbosely list archive contents [technical[all],bare]
  x             Extract files with full path

<Switches>
  -             Stop switches scanning
  @[+]          Disable [enable] file lists
  ad            Append archive name to destination path
  ag[format]    Generate archive name using the current date
  ai            Ignore file attributes
  ap<path>      Set path inside archive
  c-            Disable comments show
  cfg-          Disable read configuration
  cl            Convert names to lower case
  cu            Convert names to upper case
  dh            Open shared files
  ep            Exclude paths from names
  ep3           Expand paths to full including the drive letter
  f             Freshen files
  id[c,d,p,q]   Disable messages
  ierr          Send all messages to stderr
  inul          Disable all messages
  kb            Keep broken extracted files
  n<file>       Additionally filter included files
  n@            Read additional filter masks from stdin
  n@<list>      Read additional filter masks from list file
  o[+|-]        Set the overwrite mode
  ol[a]         Process symbolic links as the link [absolute paths]
  or            Rename files automatically
  ow            Save or restore file owner and group
  p[password]   Set password
  p-            Do not query password
  r             Recurse subdirectories
  sc<chr>[obj]  Specify the character set
  sl<size>      Process files with size less than specified
  sm<size>      Process files with size more than specified
  ta<date>      Process files modified after <date> in YYYYMMDDHHMMSS format
  tb<date>      Process files modified before <date> in YYYYMMDDHHMMSS format
  tn<time>      Process files newer than <time>
  to<time>      Process files older than <time>
  ts<m,c,a>[N]  Save or restore file time (modification, creation, access)
  u             Update files
  v             List all volumes
  ver[n]        File version control
  vp            Pause before each volume
  x<file>       Exclude specified file
  x@            Read file names to exclude from stdin
  x@<list>      Exclude files listed in specified list file
  y             Assume Yes on all queries
```
