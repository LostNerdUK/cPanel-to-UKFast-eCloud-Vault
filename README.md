# cPanel-to-UKFast-eCloud-Vault
A custom backup transport script for cPanel/WHM

This is a custom transport script that will allow cPanel/WHM backups to be sent to UKFast's eCloud Vault.

I assume the user understands the concept of access keys, secret keys, buckets, and has a bucket already set up.

## Requirements ##

In order to use this script, the [`boto`](https://github.com/boto/boto) and [`filechunkio`](https://pypi.python.org/pypi/filechunkio/) python libraries need to be installed.

The easiest way to do this is just `pip install boto filechunkio` if you have `pip` installed.

To install `pip`, run `yum install python-pip -y`

## Usage ##

1. Copy the script to someplace on the server (`/root` is probably a decent choice) and make sure it's executable
1. Go to *Additional Destinations* in the Backup Configuration in WHM and create a *Custom* destination type.
1. Give it a name and select whether you want to transfer system files
1. For `Script`, point it to the place you stuck the script (e.g. /root/cPaneltoUKFastS3.py)
1. `Backup Directory` is optional, but good to use if the bucket you're going to use already has other things
1. `Remote Host` is the **name of the bucket** you are going to use
1. `Remote Account Username` is going to be a valid access key that you have setup
1. `Remote Password` is going to be the secret key that is associated with the access key that you're using

You can get your Access Key & Secret Key from `https://my.ukfast.co.uk/ecloud-vault/integration.php`

Really, that should be it

## Known Issues ##
1. If you receive an error when validating the destination regarding not being able to find the file, make sure that in Notepad++ you have the UNIX/OSX EOL Conversion active, save the file and re-upload it. Linux likes to be picky, but don't we all?

2. Pruning old backups currently does not work. Files would need to be uploaded to S3 with the prefix "rwxsSrT-", a permission string to work correctly. I'll look into this one.

## Logging ##

This script will log operations it performs when called. When cPanel does its schedule backup runs, the logs should be located in /root/log

If cPanel backups are run manually, the log directory should exist in the present working directory that the backup is fired off from.

## Threading ##

When a file is uploaded in multiple parts, threading will be used. By default, six threads will be used, however this is easilly adjusted by changing the `THREADS` variable.
