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
At GitMachines we are interested in one-click installs to get accreditation-ready builds in order to encourage adoption.

## Status
### What our one-click build does.

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

## ToDo
See the issues.
