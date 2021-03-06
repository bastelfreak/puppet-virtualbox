# virtualbox

[![Puppet Forge](http://img.shields.io/puppetforge/v/puppet/virtualbox.svg?style=flat)](https://forge.puppetlabs.com/puppet/virtualbox) [![Build Status](https://travis-ci.org/voxpupuli/puppet-virtualbox.svg?branch=master)](https://travis-ci.org/voxpupuli/puppet-virtualbox) ![puppet-strings](http://img.shields.io/badge/docs-puppet--strings-ff69b4.svg?style=flat)

This module installs VirtualBox on a Linux host using the official repositories or custom defined repositories. By default, this module will also configure the kernel modules required to run VirtualBox.

By default, this module will install the Oracle VirtualBox yum/apt repo, install the VirtualBox package, and build the VirtualBox kernel modules. You can define a custom package name and/or version, you can also opt to not manage the repositories with this module. Because of the strange convention Oracle has opted to use for versioning VirtualBox, if you set a custom package name, the `version` parameter will be ignored. If you wish to define a package version with a custom package name, you must use the `package_ensure` parameter.

## Support

This module is tested with:

- CentOS 5.11
- CentOS 6.6
- CentOS 7.0
- Debian 6.0.10
- Debian 7.8
- Ubuntu 10.04
- Ubuntu 12.04
- Ubuntu 14.04

It may work on other distros and OS versions, but these are the versions that we're targeting. If you wish to see another distro/version added to this list, so would we! PRs are welcome :)

This module is tested with the latest version of Puppet 3 and Puppet 4; all Puppet supported versions of Ruby are included in the test matrix. If you're interested in the testing matrix, please have a look at the `.travis.yml` file in the root of the module.

## Usage

To begin using the virtualbox module, just include the virtualbox class on your node like so:

    include virtualbox

This will get you set up with the basics and will meet 90% of the use cases out there.

If you wish to manage your package repositories outside of this module, you just need to set `$manage_repo` to `false`:

    class { 'virtualbox':
      manage_repo => false,
    }

You can also specify a custom package name like so:

    class { 'virtualbox':
      manage_repo  => false,
      package_name => 'virtualbox-custom',
    }

The peculiar versioning in use by Oracle has forced us to do some funky stuff with versioning. If you're using the default package name, this module will concatenate `$package_name` and `$version` together with a dash between them. If you opt to define your own package name, the `$version` parameter is ignored completely and the only way to specify a version would be to use the `$package_ensure` parameter:

    class { 'virtualbox':
      manage_repo    => false,
      package_name   => 'virtualbox-custom',
      package_ensure => '4.3.18_96516',
    }

If you don't want to install the VirtualBox kernel extensions, you can set the `manage_kernel` parameter to `false`.

    class { 'virtualbox':
      manage_kernel => false,
    }

You can also opt to not manage the package with the `manage_package` parameter. This would effectively just install the package repository:

    class { 'virtualbox':
      manage_kernel  => false,
      manage_package => false,
    }

### Extension Pack

NOTE: To use this feature, you must have either [camptocamp/archive](https://forge.puppet.com/camptocamp/archive) or [puppet/archive](https://forge.puppet.com/puppet/archive) installed.

There's a defined type to install an Extension Pack. I'm not aware of any extension packs other than the Oracle Extension Pack, but this type should work for third party extensions. You can install Oracle's Extension Pack (adding support for USB 2.0, access to webcam, RDP and E1000 PXE ROM) like so:

    virtualbox::extpack { 'Oracle_VM_VirtualBox_Extension_Pack':
      ensure           => present,
      source           => 'http://download.virtualbox.org/virtualbox/4.3.20/Oracle_VM_VirtualBox_Extension_Pack-4.3.20.vbox-extpack',
      checksum_string  => '4b7546ddf94308901b629865c54d5840',
      follow_redirects => true,
    }

This will download the extension pack, check to make sure the downloaded file matches the expected md5sum, then install the extension pack to `/usr/lib/virtualbox/ExtensionPacks`.

## Development

1. Fork it
2. Create a feature branch
3. Write a failing test
4. Write the code to make that test pass
5. Refactor the code
6. Submit a pull request

We politely request (demand) tests for all new features. Pull requests that contain new features without a test will not be considered. If you need help, just ask!
