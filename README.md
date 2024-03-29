# cms-ars-5.0-microsoft-windows-server-2019-stig-overlay
**CMS’ ISPG (Information Security and Privacy Group) decided to discontinue funding the customization of MITRE’s Security Automation Framework (SAF) for CMS after September 2023. This repo is now in archive mode, but still accessible. For more information about SAF with current links, see https://security.cms.gov/learn/security-automation-framework-saf**


InSpec profile overlay to validate the secure configuration of Microsoft Windows Server 2019 against [DISA's](https://public.cyber.mil/stigs/) Microsoft Windows Server 2019 Security Technical Implementation Guide (STIG) Version 1, Release 3 tailored for [CMS ARS 5.0](https://security.cms.gov/library/cms-acceptable-risk-safeguards-ars) for all system categorizations.

## Getting Started  
### InSpec (CINC-auditor) setup
For maximum flexibility/accessibility, we’re moving to “cinc-auditor”, the open-source packaged binary version of Chef InSpec, compiled by the CINC (CINC Is Not Chef) project in coordination with Chef using Chef’s always-open-source InSpec source code. For more information: https://cinc.sh/

It is intended and recommended that CINC-auditor and this profile overlay be run from a __"runner"__ host (such as a DevOps orchestration server, an administrative management system, or a developer's workstation/laptop) against the target. This can be any Unix/Linux/MacOS or Windows runner host, with access to the Internet.

__For the best security of the runner, always install on the runner the _latest version_ of CINC-auditor.__ 

__The simplest way to install CINC-auditor is to use this command for a UNIX/Linux/MacOS runner platform:__
```
curl -L https://omnitruck.cinc.sh/install.sh | sudo bash -s -- -P cinc-auditor
```

__or this command for Windows runner platform (Powershell):__
```
. { iwr -useb https://omnitruck.cinc.sh/install.ps1 } | iex; install -project cinc-auditor
```
To confirm successful install of cinc-auditor:
```
cinc-auditor -v
```
> sample output:  _4.24.32_

Latest versions and other installation options are available at https://cinc.sh/start/auditor/.

## Specify your BASELINE system categorization as an environment variable
### (if undefined defaults to Moderate baseline)

```
# BASELINE (choices: Low, Low-HVA, Moderate, Moderate-HVA, High, High-HVA)
# (if undefined defaults to Moderate baseline)

on linux:
BASELINE=High

on Powershell:
$env:BASELINE="High"
```

## Tailoring to Your Environment
The following inputs must be configured in an inputs ".yml" file for the profile to run correctly for your specific environment. More information about InSpec inputs can be found in the [InSpec Profile Documentation](https://www.inspec.io/docs/reference/profiles/).

```
- Set to either the string "true" or "false"
sensitive_system: false

- List of temporary accounts on the domain
temp_accounts_domain: []

- List of temporary accounts on local system
temp_accounts_local: []

- List of emergency accounts on the domain
emergency_accounts_domain: []

- List of emergency accounts on the system
emergency_accounts_local: []

- List of authorized users in the local Administrators group for a domain controller
local_administrators_dc: []

- List of authorized users in the local Administrators group for a member server
local_administrators_member: []

- Local Administrator Account on Windows Server
local_administrator: ""

- List of authorized users in the Backup Operators Group
backup_operators: []

- List Application or Service Accounts domain
application_accounts_domain: []

- List Excluded Accounts domain
excluded_accounts_domain: []

- List Application Local Accounts
application_accounts_local: []

- List of authorized users in the local Administrators group
administrators: []

```

## Running This Overlay Directly from Github

```
# How to run (linux)
BASELINE=<your_system_categorization> cinc-auditor exec https://github.com/CMSgov/cms-ars-5.0-microsoft-windows-server-2019-stig-overlay/archive/main.tar.gz --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> -t winrm://<hostname> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json>
```


## Running This Overlay from a local Archive copy 
If your runner is not always expected to have direct access to GitHub, use the following steps to create an archive bundle of this overlay and all of its dependent tests:

(Git is required to clone the InSpec profile using the instructions below. Git can be downloaded from the [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) site.)

When the __"runner"__ host uses this profile overlay for the first time, follow these steps:

```
mkdir profiles
cd profiles
git clone https://github.com/CMSgov/cms-ars-5.0-microsoft-windows-server-2019-stig-overlay.git
cinc-auditor archive cms-ars-5.0-microsoft-windows-server-2019-stig-overlay
BASELINE=<your_system_categorization> cinc-auditor exec <name of generated archive> --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> -t winrm://<hostname> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json>
```

For every successive run, follow these steps to always have the latest version of this overlay and dependent profiles:

```
cd cms-ars-5.0-microsoft-windows-server-2019-stig-overlay
git pull
cd ..
cinc-auditor archive cms-ars-5.0-microsoft-windows-server-2019-stig-overlay --overwrite
BASELINE=<your_system_categorization> cinc-auditor exec <name of generated archive> --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> -t winrm://<hostname> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json>
```

### Different Run Options

  [Full exec options](https://docs.chef.io/inspec/cli/#options-3)

## Using Heimdall for Viewing the JSON Results

The JSON results output file can be loaded into __[heimdall-lite](https://heimdall-lite.mitre.org/)__ for a user-interactive, graphical view of the InSpec results. 

The JSON InSpec results file may also be loaded into a __[full heimdall server](https://github.com/mitre/heimdall2)__, allowing for additional functionality such as to store and compare multiple profile runs.

## Authors
* Shivani Karikar - [karikarshivani](https://github.com/karikarshivani)

## Special Thanks
* Eugene Aronne - [ejaronne](https://github.com/ejaronne)

## Contributing and Getting Help
To report a bug or feature request, please open an [issue](https://github.com/CMSgov/cms-ars-5.0-microsoft-windows-server-2019-stig-overlay/issues/new).

### NOTICE
© 2022 The MITRE Corporation.

Approved for Public Release; Distribution Unlimited. Case Number 18-3678.

### NOTICE 

MITRE grants permission to reproduce, distribute, modify, and otherwise use this software to the extent permitted by the licensed terms provided in the LICENSE.md file included with this project.
 
This software was produced by The MITRE Corporation for the U. S. Government under contract. As such the U.S. Government has certain use and data rights in this software. No use other than those granted to the U. S. Government, or to those acting on behalf of the U. S. Government, under these contract arrangements is authorized without the express written permission of The MITRE Corporation.
 
For further information, please contact The MITRE Corporation, Contracts Management Office, 7515 Colshire Drive, McLean, VA 22102-7539, (703) 983-6000.

DISA STIGs are published at: https://public.cyber.mil/stigs/
