---
layout: post
title:  "Loop OclHashcat through multiple rule files"
---

## Loop OclHashcat through multiple rule files

1. download latest https://hashcat.net/oclhashcat/#downloadlatest
2. in the extract folder create a dictionaries folder and put them in there
3. download seclists for rockyou.txt (https://github.com/danielmiessler/SecLists) and linkedin wordlist here (https://hashes.org/public.php). It's called like L1nk3d1n or something--61 million passwords. more recent password lists are better
4. run the command "dir /b" on the rules directory and save the output to rule_list.txt in the root oclhashcat directory.
5. save your hashes in the root oclhashcat directory too in hashes.txt
6. make a results folder to store your results in from the oclhashcat root
5. save this script in oclhashcat directory:
- this batch script will take all of the rule sets in the rule_list.txt and run it against the hashes.txt file and the rockyou.txt dictionary, and store the results in that folder. You can do a loop inside a loop to cycle through multiple dictionaries, but this gets most user hashes I run into with just rockyou.txt. For the next round, swap out the rockyou.txt for the linkedin dictionary

```
@echo off
for /f "tokens=*" %%a in (rule_list.txt) do (
	oclHashcat64.exe -a 0 -m 5600 -o results/%%a -r rules/%%a hashes.txt dictionaries/rockyou.txt
)
pause
```

## Loop OclHashcat through multiple dictionary and rule files.

1. follow the same install instructions from above
2. run the command "dir /b" on the dictionary directory and save the outpit to dictionary_list.txt in the root oclhashcat directory.

```
@echo off
for /f "tokens=*" %%a in (dictionary_list.txt) do (
    for /f "tokens=*" %%b in (rule_list.txt) do (
        oclHashcat64.exe -a 0 -m 5600 -o results/client.txt -r rules/%%b hashes.txt dictionaries/%%a
    )
)
pause
```

Have fun :)
