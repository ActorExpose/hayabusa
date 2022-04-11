<div align="center">
 <p>
    <img alt="Hayabusa Logo" src="hayabusa-logo.png" width="50%">
 </p>
 [ <b>English</b> ] | [<a href="README-Japanese.md">日本語</a>]
</div>

---

[tag-1]: https://img.shields.io/github/downloads/Yamato-Security/hayabusa/total?style=plastic&label=GitHub%F0%9F%A6%85DownLoads
[tag-2]: https://img.shields.io/github/stars/Yamato-Security/hayabusa?style=plastic&label=GitHub%F0%9F%A6%85Stars
[tag-3]: https://img.shields.io/github/v/release/Yamato-Security/hayabusa?display_name=tag&label=latest-version&style=plastic
[tag-4]: https://img.shields.io/badge/Black%20Hat%20Arsenal-Asia%202022-blue
[tag-5]: https://rust-reportcard.xuri.me/badge/github.com/Yamato-Security/hayabusa
[tag-6]: https://img.shields.io/badge/Maintenance%20Level-Actively%20Developed-brightgreen.svg

![tag-1] ![tag-2] ![tag-3] ![tag-4] ![tag-5] ![tag-6]

# About Hayabusa

Hayabusa is a **Windows event log fast forensics timeline generator** and **threat hunting tool** created by the [Yamato Security](https://yamatosecurity.connpass.com/) group in Japan. Hayabusa means ["peregrine falcon"](https://en.wikipedia.org/wiki/Peregrine_falcon") in Japanese and was chosen as peregrine falcons are the fastest animal in the world, great at hunting and highly trainable. It is written in [Rust](https://www.rust-lang.org/) and supports multi-threading in order to be as fast as possible. We have provided a [tool](https://github.com/Yamato-Security/hayabusa-rules/tree/main/tools/sigmac) to convert [sigma](https://github.com/SigmaHQ/sigma) rules into hayabusa rule format. The hayabusa detection rules, like sigma, are also written in YML in order to be as easily customizable and extensible as possible. It can be run either on running systems for live analysis or by gathering logs from multiple systems for offline analysis. (At the moment, it does not support real-time alerting or periodic scans.) The output will be consolidated into a single CSV timeline for easy analysis in Excel or [Timeline Explorer](https://ericzimmerman.github.io/#!index.md).

## Table of Contents

- [About Hayabusa](#about-hayabusa)
  - [Table of Contents](#table-of-contents)
  - [Main Goals](#main-goals)
    - [Threat Hunting](#threat-hunting)
    - [Fast Forensics Timeline Generation](#fast-forensics-timeline-generation)
- [Screenshots](#screenshots)
  - [Startup](#startup)
  - [Terminal Output](#terminal-output)
  - [Results Summary](#results-summary)
  - [Analysis in Excel](#analysis-in-excel)
  - [Analysis in Timeline Explorer](#analysis-in-timeline-explorer)
  - [Critical Alert Filtering and Computer Grouping in Timeline Explorer](#critical-alert-filtering-and-computer-grouping-in-timeline-explorer)
- [Sample Timeline Results](#sample-timeline-results)
- [Features](#features)
- [Planned Features](#planned-features)
- [Downloads](#downloads)
- [Compiling From Source (Optional)](#compiling-from-source-optional)
  - [Cross-compiling 32-bit Windows Binaries](#cross-compiling-32-bit-windows-binaries)
  - [macOS Compiling Notes](#macos-compiling-notes)
  - [Linux Compiling Notes](#linux-compiling-notes)
  - [Advanced: Updating Rust Packages](#advanced-updating-rust-packages)
- [Running Hayabusa](#running-hayabusa)
  - [Caution: Anti-Virus/EDR Warnings](#caution-anti-virusedr-warnings)
  - [Windows](#windows)
  - [Linux](#linux)
  - [macOS](#macos)
- [Usage](#usage)
  - [Command Line Options](#command-line-options)
  - [Usage Examples](#usage-examples)
  - [Pivot Keyword Generator](#pivot-keyword-generator)
- [Testing Hayabusa on Sample Evtx Files](#testing-hayabusa-on-sample-evtx-files)
- [Hayabusa Output](#hayabusa-output)
  - [Progress Bar](#progress-bar)
  - [Color Output](#color-output)
- [Hayabusa Rules](#hayabusa-rules)
  - [Hayabusa v.s. Converted Sigma Rules](#hayabusa-vs-converted-sigma-rules)
  - [Detection Rule Tuning](#detection-rule-tuning)
  - [Detection Level Tuning](#detection-level-tuning)
  - [Event ID Filtering](#event-id-filtering)
- [Other Windows Event Log Analyzers and Related Projects](#other-windows-event-log-analyzers-and-related-projects)
- [Windows Logging Recommendations](#windows-logging-recommendations)
- [Sysmon Related Projects](#sysmon-related-projects)
- [Community Documentation](#community-documentation)
  - [English](#english)
  - [Japanese](#japanese)
- [Contribution](#contribution)
- [Bug Submission](#bug-submission)
- [License](#license)
- [Twitter](#twitter)

## Main Goals

### Threat Hunting

Hayabusa currently has over 1300 sigma rules and around 70 hayabusa rules with more rules being added regularly. The ultimate goal is to be able to push out hayabusa agents to all Windows endpoints after an incident or for periodic threat hunting and have them alert back to a central server.

### Fast Forensics Timeline Generation

Windows event log analysis has traditionally been a very long and tedious process because Windows event logs are 1) in a data format that is hard to analyze and 2) the majority of data is noise and not useful for investigations. Hayabusa's main goal is to extract out only useful data and present it in an easy-to-read format that is usable not only by professionally trained analysts but any Windows system administrator.
Hayabusa is not intended to be a replacement for tools like [Evtx Explorer](https://ericzimmerman.github.io/#!index.md) or [Event Log Explorer](https://eventlogxp.com/) for more deep-dive analysis but is intended for letting analysts get 80% of their work done in 20% of the time. 

# Screenshots

## Startup

![Hayabusa Startup](/screenshots/Hayabusa-Startup.png)

## Terminal Output

![Hayabusa terminal output](/screenshots/Hayabusa-Results.png)


## Results Summary

![Hayabusa results summary](/screenshots/HayabusaResultsSummary.png)

## Analysis in Excel

![Hayabusa analysis in Excel](/screenshots/ExcelScreenshot.png)

## Analysis in Timeline Explorer

![Hayabusa analysis in Timeline Explorer](screenshots/TimelineExplorer-ColoredTimeline.png)

## Critical Alert Filtering and Computer Grouping in Timeline Explorer

![Critical alert filtering and computer grouping in Timeline Explorer](screenshots/TimelineExplorer-CriticalAlerts-ComputerGrouping.png)

# Sample Timeline Results

You can check out sample CSV timelines [here](https://github.com/Yamato-Security/hayabusa/tree/main/sample-results).

You can learn how to analyze CSV timelines in Excel and Timeline Explorer [here](doc/CSV-AnalysisWithExcelAndTimelineExplorer-English.pdf).

# Features

* Cross-platform support: Windows, Linux, macOS.
* Developed in Rust to be memory safe and faster than a hayabusa falcon!
* Multi-thread support delivering up to a 5x speed improvement.
* Creates a single easy-to-analyze CSV timeline for forensic investigations and incident response.
* Threat hunting based on IoC signatures written in easy to read/create/edit YML based hayabusa rules.
* Sigma rule support to convert sigma rules to hayabusa rules.
* Currently it supports the most sigma rules compared to other similar tools and even supports count rules.
* Event log statistics. (Useful for getting a picture of what types of events there are and for tuning your log settings.)
* Rule tuning configuration by excluding unneeded or noisy rules.
* MITRE ATT&CK mapping of tactics (only in saved CSV files).
* Rule level tuning.
* Create a list of unique pivot keywords to quickly identify abnormal users, hostnames, processes, etc... as well as correlate events.

# Planned Features

* Enterprise-wide hunting on all endpoints.
* Japanese language support.
* MITRE ATT&CK heatmap generation.
* User logon and failed logon summary.
* Input from JSON logs.
* JSON support for sending alerts to Elastic Stack/Splunk, etc...

# Downloads

You can download the latest stable version of hayabusa with compiled binaries from the [Releases](https://github.com/Yamato-Security/hayabusa/releases) page.

You can also `git clone` the repository with the following command and compile binary from source code:

```bash
git clone https://github.com/Yamato-Security/hayabusa.git --recursive
```

Note: If you forget to use --recursive option, the `rules` folder, which is managed as a git submodule, will not be cloned.

You can sync the `rules` folder and get latest Hayabusa rules with `git pull --recurse-submodules` or use the following command:

```bash
hayabusa.exe -u
```

If the update fails, you may need to rename the `rules` folder and try again.

# Compiling From Source (Optional)

If you have Rust installed, you can compile from source with the following command:

```bash
cargo clean
cargo build --release
```

Be sure to periodically update Rust with:

```bash
rustup update stable
```

The compiled binary will be outputted in the `target/release` folder.

## Cross-compiling 32-bit Windows Binaries

You can create 32-bit binaries on 64-bit Windows systems with the following:
```bash
rustup install stable-i686-pc-windows-msvc
rustup target add i686-pc-windows-msvc
rustup run stable-i686-pc-windows-msvc cargo build --release
```

## macOS Compiling Notes

If you receive compile errors about openssl, you will need to install [Homebrew](https://brew.sh/) and then install the following packages:
```bash
brew install pkg-config
brew install openssl
```

## Linux Compiling Notes

If you receive compile errors about openssl, you will need to install the following package.

Ubuntu-based distros:
```bash
sudo apt install libssl-dev
```

Fedora-based distros:
```bash
sudo yum install openssl-devel
```

## Advanced: Updating Rust Packages

You can update to the latest Rust crates before compiling to get the latest libraries:

```bash
cargo update
```

Please let us know if anything breaks after you update.

# Running Hayabusa 

## Caution: Anti-Virus/EDR Warnings

You may receive warning from anti-virus or EDR when trying to run hayabusa. These are false positives so you may need to configure your security products to allow running hayabusa. If you are worried about malware, please check the hayabusa source code and compile the binaries yourself.

## Windows

In Command Prompt or Windows Terminal, just run 32-bit or 64-bit Windoows binary from the hayabusa root directory. 
Example: `hayabusa-1.2.0-windows-x64.exe`

## Linux

You first need to make the binary executable. 

```bash
chmod +x ./hayabusa-1.2.0-linux-x64
```

Then run it from the Hayabusa root directory:

```bash
./hayabusa-1.2.0-linux-x64
```

## macOS

From Terminal or iTerm2, you first need to make the binary executable.

```bash
chmod +x ./hayabusa-1.2.0-mac-intel
```

Then, try to run it from the Hayabusa root directory:

```bash
./hayabusa-1.2.0-mac-intel
```

On the latest version of macOS, you may receive the following security error when you try to run it:

![Mac Error 1 EN](/screenshots/MacOS-RunError-1-EN.png)

Click "Cancel" and then from System Preferences, open "Security & Privacy" and from the General tab, click "Allow Anyway".

![Mac Error 2 EN](/screenshots/MacOS-RunError-2-EN.png)

After that, try to run it again.

```bash
./hayabusa-1.2.0-mac-intel
```

The following warning will pop up, so please click "Open".

![Mac Error 3 EN](/screenshots/MacOS-RunError-3-EN.png)

You should now be able to run hayabusa.

# Usage

## Command Line Options

```bash
USAGE:
    -d --directory=[DIRECTORY] 'Directory of multiple .evtx files.'
    -f --filepath=[FILEPATH] 'File path to one .evtx file.'
    -r --rules=[RULEFILE/RULEDIRECTORY] 'Rule file or directory. (Default: ./rules)'
    -c --color 'Output with color. (Terminal needs to support True Color.)'
    -C --config=[RULECONFIGDIRECTORY] 'Rule config folder. (Default: ./rules/config)'
    -o --output=[CSV_TIMELINE] 'Save the timeline in CSV format. (Example: results.csv)'
    -v --verbose 'Output verbose information.'
    -D --enable-deprecated-rules 'Enable rules marked as deprecated.'
    -n --enable-noisy-rules 'Enable rules marked as noisy.'
    -u --update-rules 'Update to the latest rules in the hayabusa-rules github repository.'
    -m --min-level=[LEVEL] 'Minimum level for rules. (Default: informational)'
    -l --live-analysis 'Analyze the local C:\Windows\System32\winevt\Logs folder (Windows Only. Administrator privileges required.)'
    --start-timeline=[STARTTIMELINE] 'Start time of the event logs to load. (Example: '2018/11/28 12:00:00 +09:00')'
    --end-timeline=[ENDTIMELINE] 'End time of the event logs to load. (Example: '2018/11/28 12:00:00 +09:00')'
    --rfc-2822 'Output date and time in RFC 2822 format. (Example: Mon, 07 Aug 2006 12:34:56 -0600)'
    --rfc-3339 'Output date and time in RFC 3339 format. (Example: 2006-08-07T12:34:56.485214 -06:00)'
    -U --utc 'Output time in UTC format. (Default: local time)'
    -t --thread-number=[NUMBER] 'Thread number. (Default: Optimal number for performance.)'
    -s --statistics 'Prints statistics of event IDs.'
    -q --quiet 'Quiet mode. Do not display the launch banner.'
    -Q --quiet-errors 'Quiet errors mode. Do not save error logs.'
    --level-tuning <LEVEL_TUNING_FILE> 'Tune the rule level [default: ./config/level_tuning.txt]'
    -p --pivot-keywords-list 'Create a list of pivot keywords.'
    --contributors 'Prints the list of contributors.'
```

## Usage Examples

* Run hayabusa against one Windows event log file:

```bash
hayabusa.exe -f eventlog.evtx
```

* Run hayabusa against the sample-evtx directory with multiple Windows event log files:

```bash
hayabusa.exe -d .\hayabusa-sample-evtx
```

* Export to a single CSV file for further analysis with excel or timeline explorer:

```bash
hayabusa.exe -d .\hayabusa-sample-evtx -o results.csv
```

* Only run hayabusa rules (the default is to run all the rules in `-r .\rules`):

```bash
hayabusa.exe -d .\hayabusa-sample-evtx -r .\rules\hayabusa -o results.csv
```

* Only run hayabusa rules for logs that are enabled by default on Windows:

```bash
hayabusa.exe -d .\hayabusa-sample-evtx -r .\rules\hayabusa\default -o results.csv
```

* Only run hayabusa rules for sysmon logs:

```bash
hayabusa.exe -d .\hayabusa-sample-evtx -r .\rules\hayabusa\sysmon -o results.csv
```

* Only run sigma rules:

```bash
hayabusa.exe -d .\hayabusa-sample-evtx -r .\rules\sigma -o results.csv
```

* Enable deprecated rules (those with `status` marked as `deprecated`) and noisy rules (those whose rule ID is listed in `.\rules\config\noisy_rules.txt`):

```bash
hayabusa.exe -d .\hayabusa-sample-evtx --enable-noisy-rules --enable-deprecated-rules -o results.csv
```

* Only run rules to analyze logons and output in the UTC timezone:

```bash
hayabusa.exe -d .\hayabusa-sample-evtx -r .\rules\hayabusa\default\events\Security\Logons -U -o results.csv
```

* Run on a live Windows machine (requires Administrator privileges) and only detect alerts (potentially malicious behavior):

```bash
hayabusa.exe -l -m low
```

* Create a list of pivot keywords from critical alerts and save the results. (Results will be saved to `keywords-Ip Addresses.txt`, `keywords-Users.txt`, etc...):

```bash
hayabusa.exe -l -m critical -p -o keywords
```

* Print Event ID statistics:

```bash
hayabusa.exe -f Security.evtx -s
```

* Print verbose information (useful for determining which files take long to process, parsing errors, etc...):

```bash
hayabusa.exe -d .\hayabusa-sample-evtx -v
```

* Verbose output example:

```bash
Checking target evtx FilePath: "./hayabusa-sample-evtx/YamatoSecurity/T1027.004_Obfuscated Files or Information\u{a0}Compile After Delivery/sysmon.evtx"
1 / 509 [>-------------------------------------------------------------------------------------------------------------------------------------------] 0.20 % 1s 
Checking target evtx FilePath: "./hayabusa-sample-evtx/YamatoSecurity/T1558.004_Steal or Forge Kerberos Tickets AS-REP Roasting/Security.evtx"
2 / 509 [>-------------------------------------------------------------------------------------------------------------------------------------------] 0.39 % 1s 
Checking target evtx FilePath: "./hayabusa-sample-evtx/YamatoSecurity/T1558.003_Steal or Forge Kerberos Tickets\u{a0}Kerberoasting/Security.evtx"
3 / 509 [>-------------------------------------------------------------------------------------------------------------------------------------------] 0.59 % 1s 
Checking target evtx FilePath: "./hayabusa-sample-evtx/YamatoSecurity/T1197_BITS Jobs/Windows-BitsClient.evtx"
4 / 509 [=>------------------------------------------------------------------------------------------------------------------------------------------] 0.79 % 1s 
Checking target evtx FilePath: "./hayabusa-sample-evtx/YamatoSecurity/T1218.004_Signed Binary Proxy Execution\u{a0}InstallUtil/sysmon.evtx"
5 / 509 [=>------------------------------------------------------------------------------------------------------------------------------------------] 0.98 % 1s
```

* Quiet error mode:
By default, hayabusa will save error messages to error log files.
If you do not want to save error messages, please add `-Q`.

## Pivot Keyword Generator

You can use the `-p` or `--pivot-keywords-list` option to create a list of unique pivot keywords to quickly identify abnormal users, hostnames, processes, etc... as well as correlate events. You can customize what keywords you want to search for by editing `config/pivot_keywords.txt`.
This is the default setting:

```
Users.SubjectUserName
Users.TargetUserName
Users.User
Logon IDs.SubjectLogonId
Logon IDs.TargetLogonId
Workstation Names.WorkstationName
Ip Addresses.IpAddress
Processes.Image
```

The format is `KeywordName.FieldName`. For example, when creating the list of `Users`, hayabusa will list up all the values in the `SubjectUserName`, `TargetUserName` and `User` fields. By default, hayabusa will return results from all events (informational and higher) so we highly recommend combining the `--pivot-keyword-list` option with the `-m` or `--min-level` option. For example, start off with only creating keywords from `critical` alerts with `-m critical` and then continue with `-m high`, `-m medium`, etc... There will most likely be common keywords in your results that will match on many normal events, so after manually checking the results and creating a list of unique keywords in a single file, you can then create a narrowed down timeline of suspicious activity with a command like `grep -f keywords.txt timeline.csv`.

# Testing Hayabusa on Sample Evtx Files

We have provided some sample evtx files for you to test hayabusa and/or create new rules at [https://github.com/Yamato-Security/hayabusa-sample-evtx](https://github.com/Yamato-Security/hayabusa-sample-evtx)

You can download the sample evtx files to a new `hayabusa-sample-evtx` sub-directory with the following command:

```bash
git clone https://github.com/Yamato-Security/hayabusa-sample-evtx.git
```

> Note: You need to run the binary from the Hayabusa root directory. 

# Hayabusa Output

When hayabusa output is being displayed to the screen (the default), it will display the following information:

* `Timestamp`: Default is `YYYY-MM-DD HH:mm:ss.sss +hh:mm` format. This comes from the `<Event><System><TimeCreated SystemTime>` field in the event log. The default timezone will be the local timezone but you can change the timezone to UTC with the `--utc` option.
* `Computer`: This comes from the `<Event><System><Computer>` field in the event log.
* `Event ID`: This comes from the `<Event><System><EventID>` field in the event log.
* `Level`: This comes from the `level` field in the YML detection rule. (`informational`, `low`, `medium`, `high`, `critical`) By default, all level alerts will be displayed but you can set the minimum level with `-m`. For example, you can set `-m high`) in order to only scan for and display high and critical alerts.
* `Title`: This comes from the `title` field in the YML detection rule.
* `Details`: This comes from the `details` field in the YML detection rule, however, only hayabusa rules have this field. This field gives extra information about the alert or event and can extract useful data from the `<Event><System><EventData>` portion of the log. For example, usernames, command line information, process information, etc...

When saving to a CSV file an additional two fields will be added:

* `Rule Path`: The path to the detection rule that generated the alert or event.
* `File Path`: The path to the evtx file that caused the alert or event.

## Progress Bar

The progress bar will only work with multiple evtx files.
It will display in real time the number and percent of evtx files that it has finished analyzing.

## Color Output

You can output the alerts in color based on the alert `level` by specifying `-c` or `--color`.
You can change the default colors in the config file at `./config/level_color.txt` in the format of `level,(RGB 6-digit ColorHex)`.
Note: Color can only be displayed in terminals that support [True Color](https://en.wikipedia.org/wiki/Color_depth#True_color_(24-bit)).
Example: [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/install) or [iTerm2](https://iterm2.com/) for macOS.

# Hayabusa Rules

Hayabusa detection rules are written in a sigma-like YML format and are located in the `rules` folder. In the future, we plan to host the rules at [https://github.com/Yamato-Security/hayabusa-rules](https://github.com/Yamato-Security/hayabusa-rules) so please send any issues and pull requests for rules there instead of the main hayabusa repository.

Please read [the hayabusa-rules repository README](https://github.com/Yamato-Security/hayabusa-rules/blob/main/README.md) to understand about the rule format and how to create rules.

All of the rules from the hayabusa-rules repository should be placed in the `rules` folder.
`informational` level rules are considered `events`, while anything with a `level` of `low` and higher are considered `alerts`.

The hayabusa rule directory structure is separated into 3 directories:

* `default`: logs that are turned on in Windows by default.
* `non-default`: logs that need to be turned on through group policy, security baselines, etc...
* `sysmon`: logs that are generated by [sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon).
* `testing`: a temporary directory to put rules that you are currently testing

Rules are further seperated into directories by log type (Example: Security, System, etc...) and are named in the following format: 

* Alert format: `<EventID>_<EventDescription>_<AttackDescription>.yml`
* Alert example: `1102_SecurityLogCleared_PossibleAntiForensics.yml`
* Event format: `<EventID>_<EventDescription>.yml`
* Event example: `4776_NTLM-LogonToLocalAccount.yml`

Please check out the current rules to use as a template in creating new ones or for checking the detection logic.

## Hayabusa v.s. Converted Sigma Rules

Sigma rules need to first be converted to hayabusa rule format explained [here](https://github.com/Yamato-Security/hayabusa-rules/blob/main/tools/sigmac/README.md). Hayabusa rules are designed solely for Windows event log analysis and have the following benefits:

1. An extra `details` field to display additional information taken from only the useful fields in the log.
2. They are all tested against sample logs and are known to work. 
   > Some sigma rules may not work as intended due to bugs in the conversion process, unsupported features, or differences in implementation (such as in regular expressions).

**Limitations**: To our knowledge, hayabusa provides the greatest support for sigma rules out of any open source Windows event log analysis tool, however, there are still rules that are not supported:

1. Rules that use regular expressions that do not work with the [Rust regex crate](https://docs.rs/regex/1.5.4/regex/)
2. Aggregation expressions besides `count` in the [sigma rule specification](https://github.com/SigmaHQ/sigma/wiki/Specification).
3. Rules that use `|near`.

## Detection Rule Tuning

Like firewalls and IDSes, any signature-based tool will require some tuning to fit your environment so you may need to permanently or temporarily exclude certain rules.

You can add a rule ID (Example: `4fe151c2-ecf9-4fae-95ae-b88ec9c2fca6`) to `rules/config/exclude_rules.txt` in order to ignore any rule that you do not need or cannot be used.

You can also add a rule ID to `rules/config/noisy_rules.txt` in order to ignore the rule by default but still be able to use the rule with the `-n` or `--enable-noisy-rules` option.

## Detection Level Tuning

Hayabusa and Sigma rule authors will determine the risk level of the alert when writing their rules.
However, the actual risk level will differ between environments.
You can tune the risk level of the rules by adding them to `./config/level_tuning.txt` and executing `hayabusa.exe --level-tuning` which will update the `level` line in the rule file.
Please note that the rule file will be updated directly.

`./config/level_tuning.txt` sample line:

```
id,new_level
00000000-0000-0000-0000-000000000000,informational # sample level tuning line
```

In this case, the risk level of the rule with an `id` of `00000000-0000-0000-0000-000000000000` in the rules directory will have its `level` rewritten to `informational`.

## Event ID Filtering

You can filter on event IDs by placing event ID numbers in `config/target_eventids.txt`.
This will increase performance so it is recommended if you only need to search for certain IDs.

We have provided a sample ID filter list at [`config/target_eventids_sample.txt`](https://github.com/Yamato-Security/hayabusa/blob/main/config/target_eventids_sample.txt) created from the `EventID` fields in all of the rules as well as IDs seen in actual results.

Please use this list if you want the best performance but be aware that there is a slight possibility for missing events (false negatives). 

# Other Windows Event Log Analyzers and Related Projects

There is no "one tool to rule them all" and we have found that each has its own merits so we recommend checking out these other great tools and projects and seeing which ones you like.

* [APT-Hunter](https://github.com/ahmedkhlief/APT-Hunter) - Attack detection tool written in Python.
* [Awesome Event IDs](https://github.com/stuhli/awesome-event-ids) -  Collection of Event ID resources useful for Digital Forensics and Incident Response
* [Chainsaw](https://github.com/countercept/chainsaw) - A similar sigma-based attack detection tool written in Rust.
* [DeepBlueCLI](https://github.com/sans-blue-team/DeepBlueCLI) - Attack detection tool written in Powershell by [Eric Conrad](https://twitter.com/eric_conrad).
* [Epagneul](https://github.com/jurelou/epagneul) - Graph visualization for Windows event logs.
* [EventList](https://github.com/miriamxyra/EventList/) - Map security baseline event IDs to MITRE ATT&CK by [Miriam Wiesner](https://github.com/miriamxyra).
* [EvtxECmd](https://github.com/EricZimmerman/evtx) - Evtx parser by [Eric Zimmerman](https://twitter.com/ericrzimmerman).
* [EVTXtract](https://github.com/williballenthin/EVTXtract) - Recover EVTX log files from unallocated space and memory images.
* [EvtxToElk](https://www.dragos.com/blog/industry-news/evtxtoelk-a-python-module-to-load-windows-event-logs-into-elasticsearch/) - Python tool to send Evtx data to Elastic Stack.
* [EVTX ATTACK Samples](https://github.com/sbousseaden/EVTX-ATTACK-SAMPLES) - EVTX attack sample event log files by [SBousseaden](https://twitter.com/SBousseaden).
* [EVTX-to-MITRE-Attack](https://github.com/mdecrevoisier/EVTX-to-MITRE-Attack) - Another great repository of EVTX attack sample logs mapped to ATT&CK.
* [EVTX parser](https://github.com/omerbenamram/evtx) - the Rust library we used written by [@OBenamram](https://twitter.com/obenamram).
* [LogonTracer](https://github.com/JPCERTCC/LogonTracer) - A graphical interface to visualize logons to detect lateral movement by [JPCERTCC](https://twitter.com/jpcert_en).
* [RustyBlue](https://github.com/Yamato-Security/RustyBlue) - Rust port of DeepBlueCLI by Yamato Security.
* [Sigma](https://github.com/SigmaHQ/sigma) - Community based generic SIEM rules.
* [so-import-evtx](https://docs.securityonion.net/en/2.3/so-import-evtx.html) - Import evtx files into Security Onion.
* [SysmonTools](https://github.com/nshalabi/SysmonTools) - Configuration and off-line log visualization tool for Sysmon.
* [Timeline Explorer](https://ericzimmerman.github.io/#!index.md) - The best CSV timeline analyzer by [Eric Zimmerman](https://twitter.com/ericrzimmerman).
* [Windows Event Log Analysis - Analyst Reference](https://www.forwarddefense.com/media/attachments/2021/05/15/windows-event-log-analyst-reference.pdf) - by Forward Defense's Steve Anson.
* [WELA (Windows Event Log Analyzer)](https://github.com/Yamato-Security/WELA) - The swiff-army knife for Windows event logs by [Yamato Security](https://github.com/Yamato-Security/)
* [Zircolite](https://github.com/wagga40/Zircolite) - Sigma-based attack detection tool written in Python.

# Windows Logging Recommendations

In order to properly detect malicious activity on Windows machines, you will need to improve the default log settings. We recommend the following sites for guidance:
* [JSCU-NL (Joint Sigint Cyber Unit Netherlands) Logging Essentials](https://github.com/JSCU-NL/logging-essentials)
* [ACSC (Australian Cyber Security Centre) Logging and Fowarding Guide](https://www.cyber.gov.au/acsc/view-all-content/publications/windows-event-logging-and-forwarding)
* [Malware Archaeology Cheat Sheets](https://www.malwarearchaeology.com/cheat-sheets)

# Sysmon Related Projects

To create the most forensic evidence and detect with the highest accuracy, you need to install sysmon. We recommend the following sites:
* [Sysmon Modular](https://github.com/olafhartong/sysmon-modular)
* [TrustedSec Sysmon Community Guide](https://github.com/trustedsec/SysmonCommunityGuide)

# Community Documentation

## English

* 2022/01/24 [Graphing Hayabusa results in neo4j](https://www.youtube.com/watch?v=7sQqz2ek-ko) by Matthew Seyer ([@forensic_matt](https://twitter.com/forensic_matt))

## Japanese

* 2022/01/22 [Visualizing Hayabusa results in Elastic Stack](https://qiita.com/kzzzzo2/items/ead8ccc77b7609143749) by [@kzzzzo2](https://qiita.com/kzzzzo2)
* 2021/12/31 [Intro to Hayabusa](https://itib.hatenablog.com/entry/2021/12/31/222946) by itiB ([@itiB_S144](https://twitter.com/itiB_S144))
* 2021/12/27 [Hayabusa internals](https://kazuminkun.hatenablog.com/entry/2021/12/27/190535) by Kazuminn ([@k47_um1n](https://twitter.com/k47_um1n))

# Contribution

We would love any form of contribution. Pull requests, rule creation and sample evtx logs are the best but feature requests, notifying us of bugs, etc... are also very welcome.

At the least, if you like our tool then please give us a star on Github and show your support!

# Bug Submission

Please submit any bugs you find [here.](https://github.com/Yamato-Security/hayabusa/issues/new?assignees=&labels=bug&template=bug_report.md&title=%5Bbug%5D)
This project is currently actively maintained and we are happy to fix any bugs reported.

# License

Hayabusa is released under [GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html) and all rules are released under the [Detection Rule License (DRL) 1.1](https://github.com/SigmaHQ/sigma/blob/master/LICENSE.Detection.Rules.md).

# Twitter

You can recieve the latest news about Hayabusa, rule updates, other Yamato Security tools, etc... by following us on Twitter at [https://twitter.com/SecurityYamato](@SecurityYamato).