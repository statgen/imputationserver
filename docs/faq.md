# Frequently Asked Questions

## I did not receive a password for my imputation job
The TOPMed Imputation Server creates a random password for each imputation job. This password is not stored on server-side at any time. If you didn't receive a password, please check your Mail SPAM folder. Please note that we are not able to re-send you the password. If you lose it, you will need to re-run your imputation job.  

## I would like to impute more than 25,000 samples
Due to server resource requirements, the Imputation Server will not accept jobs that are very small or very large (< 20 or > 25,000 samples).

This limit exists to preserve quality of service for a wide audience. A workaround is to break large jobs into multiple chunks of 25k samples each. After completion, the results can be re-merged using [hds-util](https://github.com/statgen/hds-util) to combine the chunks and calculate the corrected R2.

If you have a use case that routinely requires smaller or larger jobs, please [contact us](/contact) with details.

## I would like to impute WGS data, but the server says I have too many variants
The Imputation Server is meant to fill in the gaps between sites on a genotyping array (not WGS). Due to server resource requirements, imputing samples with many variants (more than 20,000 in any given 10Mb chunk) is not supported.

If you have WGS at 50x, then you would get little to no benefit from imputing on the server.

## How many jobs can I submit at once?
There is a limit of three concurrent jobs per person. The TOPMed imputation server is a free resource, and these limits allow us to provide service to a wide audience. Please do not attempt to bypass these limits by creating multiple accounts. We monitor usage, and reserve the right to terminate jobs or accounts that are in violation of the policy.

## Unzip command is not working
Please check the following points: (1) When selecting AES256 encryption, please use [7z](https://www.7-zip.org/download.html) to unzip your files (Debian: `sudo apt-get install p7zip-full`). For the default encryption option, all common `.zip` decompression programs should work. (2) If your password includes special characters (e.g. \\), please put single or double quotes around the password when extracting it from the command line (e.g. `7z x -p"PASSWORD" chr_22.zip`).

## Extending expiration date or reset download counter
Your data is available for 7 days. If you need an extension, please [let us know](/contact).

## How can I improve the download speed?
[aria2](https://aria2.github.io/) tries to utilize your maximum download bandwidth. Remember to raise the k parameter significantly (-k, --min-split-size=SIZE). You will otherwise hit the Imputation Server download limit for each file (thanks to Anthony Marcketta for pointing this out).

## Can I download all results at once?
We provide wget command for all results. Please open the results tab. The last column in each row includes direct links to all files.

## Is this service secure?
Due to small team size, it is difficult for us to complete detailed security questionnaires for every company or entity. However, as of May 2023, we have completed a rigorous external security review and received federal Authorization to Operate (ATO) from NHLBI/NIH. Please see our [security](/data-sensitivity) documentation for some common information, or [contact us](/contact) for specific questions. 
