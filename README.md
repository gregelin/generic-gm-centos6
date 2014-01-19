# Status

| Date         | Status |
|--------------|-------------|
| Jan 18, 2014 | VM OK.   |

For additional status visit [STATUS.md](STATUS.md)

# About

This repo is to create a Generic GitMachines CentOS 6 that is the starting point for building other GitMachines.

# Versions

## 
| Version | Description |
|---------|-------------|
| v0.1 nice | one-click install to working CentOS box with ports 8080 and 8081 properly opened. |

## How can I contribute?
We are learning as we go and do not yet clear asks to make of others. However, you can:
- Follow along, try things, and submit issues
- Fork, hack, and make pull requests (PLEASE keep these small for now and related to our project goals).

## Why this project?
The current [statedecoded-vagrant](https://github.com/statedecoded/statedecoded-vagrant) is helpful in setting up the environment on vagrant, but is not yet a one-click install. There are also has some gotchas we found in using it.

At GitMachines we are interested in one-click installs to get accreditation-ready builds in order to encourage adoption.

## Status
### What our one-click build does..

1. Uses CentOS, which is very very close to RedHat Enterprise
2. Configures CentOS firewall for Apache and Tomcat

### What user needs to do...
1. Clone repo and cd into repo directory
2. Type `vagrant up`
3. Surf web for 12-15 minutes
4. Type `vagrant ssh` to log into your CentOS Gitmachine

## Dependencies
  * Latest version of vagrant (vagrantup.com)
  * Latest version Virtualbox (4.2.10 guest additions on our base box)
  * Do not have service running on ports 8080 or 8081 on host computer.

## Instructions

### One-click build and (simple) audit run
```
  # Clone this repo locally to your computer and switch to repo directory.
  git clone git@github.com:GitMachines/generic-gm-centos6.git
  cd generic-gm-centos6
  
  # Stop any running virtual machines that might conflict on ports 8080 and 8081.
  # Launch your gitmachine 
  vagrant up
  # Browse the web, b/c this will take a while. 

  # Your generic GitMachines is running - BUT HAS NOTHING ON IT.
  # Openscap has been installed and a very (very) simple scan is run 

  # Check out your GitMachine!
  open audit/home.html

```

### (Optional) SSH into your gitmachine and run the SCAP test manually
You can run your own audit checks using installed openscap `oscap` from the command line steps.

``` 
  vagrant ssh
  
  # Re-run sample scap script
  /vagrant/resources/scripts/oscap-rhel6.sh

  # Reports are available in audit/reports directory.

  # Want to run your own scan, here is the command format from oscap-rhel6.sh

oscap xccdf eval --profile stig-rhel6-server \
  --results /vagrant/audit/reports/results-stig-rhel6-server.xml \
  --report /vagrant/audit/reports/report-stig-rhel6-server.html \
  --cpe /usr/share/xml/scap/ssg/content/ssg-rhel6-cpe-dictionary.xml \
  /usr/share/xml/scap/ssg/content/ssg-rhel6-xccdf.xml

```


### (Recommended) Install Virginia State Laws
Your statedecoded will look a bit lame without any laws. We've pre-configured everything to use Virginia's laws as a sample.

To finish the import of Virginia's Laws, open web browser and navigate to `http://localhost:8080/admin/` and follow instructions to import.

### (Optional) Install Laws for a Different State
To use laws of a different state, follow the steps below to modify files and re-import laws to use a different state laws.

1. Adjust config.inc.php settings. See http://statedecoded.github.io/documentation/config.html
2. Rename `statedecoded/includes/class.Virginia.inc` to new state name, example `statedecoded/includes/class.Indiana.inc`.
3. Prepare the laws to the StateDecoded XML format. See http://statedecoded.github.io/documentation/
4. Replace the Virginia XML law files with new state XML formatted laws into `statedecoded/htdocs/admin/import-data/code/`. 
5. Empty the database. (Better instructions to come.)
6. Re-import the laws from the admin page at `http://localhost:8080/admin/`.

### (Optional) Give your GitMachine a domain name of statedecoded.dev
Your GitMachine and Statedecoded website is configured to be accessed by the domain `statedecoded.dev`. To do this, add the following line to the bottome of your host computer's known hosts file (ex: `/etc/hosts` on Linux and Macs).

```
192.168.56.101  statedecoded.dev
```

Note: We do not automate changing your host computer's known hosts file because it is highly risky to do automatically.

## Security

This is box is being tested for the following security

- http://repos.fedorapeople.org/repos/scap-security-guide/epel-6-scap-security-guide.repo
- http://repos.fedorapeople.org/repos/gitopenscap/openscap/epel-6-openscap.repo

## Why CentOS instead of Ubuntu?
- Bit compatibility with RedHat Enterprise Linux (RHEL) since RHEL is popular with is common/popular among governments and businesses.
- We want cities to adopt Statedecoded and RHEL has government acceptance b/c it is built on SELinux (Security Enhanced Linux) 
- SELinux has elements, like default firewall (/etc/sysconfig/iptables) that Ubuntu does not
- OpenSCAP (Security Content Automation Protocols) and base line control configurations already exist for CentOS but do not yet for Ubuntu (from what we can tell). We need SCAP to produce the scans and audit reports to make Statedecoded accreditation-ready. 

## Why from scratch?
Why not just start from what [statedecoded-vagrant](https://github.com/statedecoded/statedecoded-vagrant) has?
- To learn.
- To deal easier with CentOS's built-in firewall.
- To automate OpenSCAP scanning and reporting.
- To see if we can streamline and further automate the install.
- To rethink how documentation can be managed and even driven from code.
- Because some installations will require the database to be run on a different server from the application and to have other redundancies. We want to understand how to create a path for varying configurations.

## ToDo
See the issues.
