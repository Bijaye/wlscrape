#WLScrape.py - A tool for scrapping the possible malware from the Wikileaks AKP leak

##Introduction

[Wikileaks](https://www.wikileaks.org) has released a large set of e-mails leaked from the Turkish party AKP. Unfortunately, no processing of any kind has been performed on these e-mails - they are just a raw dump. Since many of the AKP members have been recipients of malware sent by e-mail (most likely random spam but could have also been targeted attacks), the received malware in the e-mails is also present in the dump. As a result, the Wikileaks site is hosting malware, which leads to various sites like Google and Facebook blocking it. For the record, I consider this to be extremely irresponsible on the part of Wikileaks. Malware distribution is __not__ "journalism" by _any_ definition of the term.

This script was written for the purpose of getting information about the attached files with suspicious extensions, so that they could be scanned - either by downloading them and scanning them locally, or by obtaining their MD5 hashes and submitting those to [VirusTotal](https://www.virustotal.com/).

##Installation

The script depends on the several Python modules (it should work both in Python 2.6+ and 3.x), which are not part of the default installation, so they have to be installed before the script is able to run. They can all be installed via the command

	pip install module_name

If you are on Linux and want to install the module system-wide, use

	sudo pip install module_name

In particular, the following modules have to be installed:

	lxml
	requests
	json
	wget

In addition, if the script produces bizarre `InsecurePlatform` errors, you should run the command

	pip install "requests[security]"

The above is for Windows. If you use a different command shell (e.g., `bash` or `zsh` on Linux), you might need to escape the brackets in a different way, e.g.

	sudo pip install requests\[security\]

##Usage

The script takes as a command-line argument one or more file extensions. It fetches information from the Wikileaks site (and the AKP e-mail dump area on it, in particular) about the e-mail file attachments matching these extensions. By default, it outputs a JSON array containing the URL where the file resides, the MD5 hash of the file, and the extension of the file. The set of output files will be unique by MD5 - that is, no two elements will have the same MD5 hash, even if multiple e-mails containing the file exist on the Wikileaks site (if they do, only the first found URL will be present).

Suggested extensions that might contain malware are: `docm`, `exe`, `jar`, `ace`, `arj`, `cab`, `gz`, `js`, `pps`, `ppt`, `rar`, `rtf`, `pdf`, `zip`. Attention: the `pdf` and `zip` extensions will result in thousands of files!

###Command-line options

`-h`, `--help`	Displays a short explanation how to use the script and what the command-line options are.

`-v`, `--version`	Displays the version of the script.

`-m`, `--md5only`	Instead of a JSON array, the script outputs only the MD5 hashes of the files, one per line, in upper case.

`-d`, `--download`	The script downloads the files that match the specified extension(s). The files are saved in the current directory with a name equal to their MD5 hash in upper case (and not the original name in the e-mail attachment, in order to prevent different files with the same names from overwriting each other) and extension equal to the matching extension.

##Change log

Version 1.00	Initial version.
