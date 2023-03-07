We use [Buddy](https://buddy.works) to handle deploying sites from local to staging/production.

Once you have made an initial git commit to your project repo, you will be able to set up a new project in Buddy linked to this repo. 

The MWB Modules Blank theme comes with a buddy.yaml configuration file in the theme root, which is pre-configured with two pipelines for deploying to staging and production servers.\
This file will need to be updated with the server information for your project. In the file, for both pipelines there are some SSH variables defined as follows:

<span dir="">`variables`</span>`:`\
`-` <span dir="">`key`</span>`:` <span dir="">`"ssh_hostname"`\
   `value`</span>`:` <span dir="">`"77.72.5.95"`\
   `type`</span>`:` <span dir="">`"VAR"`\
</span>`-` <span dir="">`key`</span>`:` <span dir="">`"ssh_password"`\
   `value`</span>`:` <span dir="">`""`\
   `type`</span>`:` <span dir="">`"VAR"`\
   `encrypted`</span>`:` <span dir="">`true`\
</span>`-` <span dir="">`key`</span>`:` <span dir="">`"ssh_port"`\
   `value`</span>`:` <span dir="">`"722"`\
   `type`</span>`:` <span dir="">`"VAR"`\
</span>`-` <span dir="">`key`</span>`:` <span dir="">`"ssh_username"`\
   `value`</span>`:` <span dir="">`""`\
   `type`</span>`:` <span dir="">`"VAR"`</span>

The hostname and port are set to the default for our Krystal VPS. Once you create a new account on WHM for the staging/production site, the username and password variables here will need to be set to the cPanel account username and password.

**Note: The ssh_password variable needs to be encrypted, to do this you will need to go to your project on Buddy, then go to Project Settings -> YAML tools dropdown -> Encrypt sensitive value for YAML. Enter the cPanel account password in here, then use the encrypted value provided for the YAML file.**

Once you have set these variables and pushed to git, your staging/production deployments should be ready to go. 

The deployment pipelines will perform the following tasks:

* Check if WordPress is installed on the account, and otherwise install the latest version
* Create a MySQL database and user on the account, and configure the WordPress install to use this database
* Check if the base theme is installed on the site, and otherwise download and install the latest version from Gitlab
* Upload the latest child theme files from Gitlab to a new release
* Run NPM on the child theme release, then clean up the node_modules directory (to save disk space)
* Switch the child theme directory to point to the new release, which will make the new release live on the site