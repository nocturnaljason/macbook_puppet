# Configure EPEL (Extra Repository for Enterprise Linux)

# Note

This is the last release that will support anything before Puppet 4.

# About
This module basically just mimics the epel-release rpm. The same repos are
enabled/disabled and the GPG key is imported.  In the end you will end up with
the EPEL repos configured.

The following Repos will be setup and enabled by default:

  * epel

Other repositories that will setup but disabled (as per the epel-release setup)

  * epel-debuginfo
  * epel-source
  * epel-testing
  * epel-testing-debuginfo
  * epel-testing-source

# Usage

In nearly all cases, you can simply _include epel_ or classify your nodes with
the epel class. There are quite a few parameters available if you need to modify
the default settings for the epel repository such as having your own mirror, an
http proxy, or disable gpg checking.

You can also use a puppet one-liner to get epel onto a system.

    puppet apply -e 'include epel'

# Proxy
If you have a http proxy required to access the internet, you can use either
a class parameter in the _epel_ class (epel_proxy), or edit the $proxy variable
in the params.pp file. By default no proxy is assumed.

# Why?
I am a big fan of EPEL. I actually was one of the people who helped get it
going. I am also the owner of the epel-release package, so in general this
module should stay fairly up to date with the official upstream package.

I just got sick of coding Puppet modules and basically having an assumption
that EPEL was setup or installed. I can now depend on this module instead.

I realize it is fairly trivial to get EPEL setup. Every now-and-then however
the path to epel-release changes because something changes in the package (mass
rebuild, rpm build macros updates, etc). This module will bypass the changing
URL and just setup the package mirrors.

This does mean that if you are looking for RPM macros that are normally
included with EPEL release, this will not have them.

# Further Information

* [EPEL Wiki](http://fedoraproject.org/wiki/EPEL)
* [epel-release package information](http://mirrors.servercentral.net/fedora/epel/6/i386/repoview/epel-release.html)

# ChangeLog

=======

1.3.1
 * Remove an Epel::Rpm_gpg_key collector that could cause circular dependencies

1.3.0
 * Add ability to disable and not define any resources from this module. This is useful if another module pulls in this module, but you already have epel managed another way.
 * Ability to specify your own TLS certs
 * repo files are now templated instead of sourced.
 * properly use metalink vs mirrorlist

 1.2.2
 * Add dep on stdlib for getvar function call

 1.2.1
 * Minor fix that lets facter 1.6 still work
 * Enforce strict variables

  1.2.0
  * Rework testing to use TravisCI
  * If you specify a baseurl, disable mirrorlist

  1.1.1
  * Ensure that GPG keys are using short IDs (issue #33)

  1.1.0
  * Default URLs to be https
  * Add ability to include/exclude packages

  1.0.2
  * Update README with usage section.
  * Fix regression when os_maj_version fact was required
  * Ready for 1.0 - replace Modulefile with metadata.json
  * Replace os_maj_version custom fact with operatingsystemmajrelease
  * Works for EPEL7 now as well.

# Testing

  * This is commonly used on Puppet Enterprise 3.x
  * This was tested using Puppet 3.3.0 on Centos5/6
  * This was tested using Puppet 3.1.1 on Amazon's AWS Linux
  * This was tested using Puppet 3.8 and Puppet 4 now as well!
  * Note Ruby 2.2 and Puppet 3.8 are not yet friends.
  * I assume it will work on any RHEL variant (Amazon Linux is debatable as a variant)
  * Amazon Linux compatability not promised, as EPEL doesn't always work with it.

# Lifecycle

  * No functionality has been introduced that should break Puppet 2.6 or 2.7, but I am no longer testing these versions of Puppet as they are end-of-lifed from Puppet Labs.
  * This also assumes a facter of greater than 1.7.0 -- at least from a testing perspective.
  * I'm not actively fixing bugs for anything in facter < 2 or puppet < 3.8

## Unit tests

Install the necessary gems

    bundle install --path vendor --without system_tests

Run the RSpec and puppet-lint tests

    bundle exec rake test

## System tests

If you have Vagrant >=1.1.0 you can also run system tests:

    RSPEC_SET=centos-64-x64 bundle exec rake spec:system

Available RSPEC_SET options are in .nodeset.yml

# License
Apache Software License 2.0

# Author/Contributors
 * Aaron <slapula@users.noreply.github.com>
 * Alex Harvey <Alex_Harvey@amp.com.au>
 * Chad Metcalf <metcalfc@gmail.com>
 * Ewoud Kohl van Wijngaarden <e.kohlvanwijngaarden@oxilion.nl>
 * Jeffrey Clark <jclark@nmi.com>
 * Joseph Swick <joseph.swick@meltwater.com>
 * Matthaus Owens <mlitteken@gmail.com>
 * Michael Hanselmann <hansmi@vshn.ch>
 * Michael Stahnke <stahnma@fedoraproject.org>
 * Michael Stahnke <stahnma@puppet.com>
 * Michael Stahnke <stahnma@puppetlabs.com>
 * Michael Stahnke <stahnma@websages.com>
 * Micka??l Can??vet <mickael.canevet@camptocamp.com>
 * Nick Le Mouton <nick@noodles.net.nz>
 * Pro Cabales <proletaryo@gmail.com>
 * Proletaryo Cabales <proletaryo@gmail.com>
 * Riccardo Calixte <rcalixte@broadinstitute.org>
 * Robert Story <rstory@localhost>
 * Rob Nelson <rnelson0@gmail.com>
 * Stefan Goethals <stefan@zipkid.eu>
 * Tim Rupp <caphrim007@gmail.com>
 * Toni Schmidbauer <toni@stderr.at>
 * Trey Dockendorf <treydock@gmail.com>
 * Troy Bollinger <troy@us.ibm.com>
 * Vlastimil Holer <holer@ics.muni.cz>

# Alternatives
If you're on CentOS 7, you can just `yum install epel-release` as it's in centos-extras.
