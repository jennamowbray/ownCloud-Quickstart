# Owncloud Quickstart Guide
The purpose of this guide is to provide administrators with the information that they need to install and configure an ownCloud server and to provide users with the information that they need to connect to the ownCloud server using the desktop client or mobile app.

# Administrators

## Installation

There are three ways that you can install the ownCloud server: 
- Docker 
- Manual installation
- Command line

### Prerequisites
See the [ownCloud Server Admin Manual](https://doc.owncloud.com/server/10.2/admin_manual/installation/manual_installation.html#prerequisites "ownCloud Administrator Guide") for a complete list of prerequisites.



### Docker installation
ownCloud can be installed using Docker, using the [official ownCloud Docker image](https://hub.docker.com/r/owncloud/server/ "official ownCloud Docker image"). This official image is designed to work with a data volume in the host file system and with separate MariaDB and Redis containers. This configuration:

- Exposes ports 8080, allowing for HTTP connections.

- Mounts the data and MySQL data directories on the host for persistent storage.

##### Installation on a local machine
1. Create a new project directory.
2. Download `docker-compose.yml` from the ownCloud Docker GitHub repository into the new directory.
3. Create a `.env` configuration file that contains the following required configuration settings:

| Setting Name     	| Description               	| Example   	|
|------------------	|---------------------------	|-----------	|
| OWNCLOUD_VERSION 	| The ownCloud version      	| latest    	|
| OWNCLOUD_DOMAIN  	| The ownCloud domain       	| localhost 	|
| ADMIN_USERNAME   	| The admin username        	| admin     	|
| ADMIN_PASSWORD   	| The admin user's password 	| admin     	|
| HTTP_PORT        	| The HTTP port to bind to  	| 8080      	|

4. Start the container using your preferred Docker command-line tool.
5. When the process completes, check that all containers successfully started by running **docker-compose ps**.


### Manual Installation on Linux
#### Install ownCloud

1. Go to the ownCloud Download Page.
2. Go to Download ownCloud Server > Download > Archive file for server owners and download either the tar.bz2 or .zip archive.
This downloads a file named owncloud-x.y.z.tar.bz2 or owncloud-x.y.z.zip (where x.y.z is the version number).

3. Download its corresponding checksum file, e.g., owncloud-x.y.z.tar.bz2.md5 or owncloud-x.y.z.tar.bz2.sha256.

4. Verify the MD5 or SHA256 sum:
> md5sum -c owncloud-x.y.z.tar.bz2.md5 < owncloud-x.y.z.tar.bz2
sha256sum -c owncloud-x.y.z.tar.bz2.sha256 < owncloud-x.y.z.tar.bz2
md5sum  -c owncloud-x.y.z.zip.md5 < owncloud-x.y.z.zip
sha256sum  -c owncloud-x.y.z.zip.sha256 < owncloud-x.y.z.zip

5. Verify the PGP signature:
> wget https://download.owncloud.org/community/owncloud-x.y.z.tar.bz2.asc
wget https://owncloud.org/owncloud.asc
gpg --import owncloud.asc
gpg --verify owncloud-x.y.z.tar.bz2.asc owncloud-x.y.z.tar.bz2

6. Extract the archive contents. Run the appropriate unpacking command for your archive type:
> tar -xjf owncloud-x.y.z.tar.bz2
unzip owncloud-x.y.z.zip

7. Copy the single ownCloud directory to its final destination. 
If you are running the Apache HTTP server, you can safely install ownCloud in your Apache document root:
> cp -r owncloud /path/to/webserver/document-root

	Where `/path/to/webserver/document-root` is replaced by the document root of your Web server:

	> cp -r owncloud /var/www
	
	On other HTTP servers, it is recommended to install ownCloud outside of the document root.

#### Configure Apache
On Debian, Ubuntu, and their derivatives, Apache installs with a useful configuration, so all you have to do is create an `/etc/apache2/sites-available/owncloud.conf` file with these lines in it, replacing the Directory and other file paths with your own file paths:
    `Alias /owncloud "/var/www/owncloud/"

    <Directory /var/www/owncloud/>
      Options +FollowSymlinks
      AllowOverride All

     <IfModule mod_dav.c>
      Dav off
     </IfModule>
    </Directory>`
   
  Then, create a symlink to /etc/apache2/sites-enabled:

    ln -s /etc/apache2/sites-available/owncloud.conf /etc/apache2/sites-enabled/owncloud.conf

#### Run the Installation Wizard
**Important**: We **strongly** encourage you to protect the Installation Wizard through some form of password authentication or access control. If the installer is left unprotected when exposed to the public internet, there is the possibility that a malicious actor could finish the installation and block you out — or worse. So please ensure that only you — or someone from your organization — can access the web installer.

When the ownCloud prerequisites are fulfilled and all ownCloud files are installed, the last step to completing the installation is running the Installation Wizard.

1. Point your web browser to http://localhost/owncloud.

2. Enter the administrator’s username and password.

3. Click **Finish Setup**.

For additional information, see [In-Depth Guide](http://https://doc.owncloud.com/server/10.2/admin_manual/installation/installation_wizard.html "In-Depth Guide").

### Command line installation

ownCloud can be installed entirely from the command line. This installation method is convenient for scripted operations and for systems administrators who prefer using the command line over a GUI.

1. Ensure your server meets the ownCloud prerequisites.

2.  [Download the source](https://owncloud.com/download-server/#instructions-server "Download the source") (whether community or enterprise) directly from ownCloud, and then unpack (decompress) the tarball into the appropriate directory.

3. Set your Web Server user to be the owner of the unpacked ownCloud directory, as in the example below.
> `$ sudo chown -R www-data:www-data /var/www/owncloud/`

4. Use the `occ command`, from the root directory of the ownCloud source, to perform the installation. 
Doing this removes the need to run the Graphical Installation Wizard.

5. When the command completes, apply the correct permissions to your ownCloud files and directories.
	**Note**: This step is extremely important, as it helps protect your ownCloud installation and ensure that it will operate correctly.




## Create user accounts

1. At the top of the ownCloud **Users** page,  enter the new user’s Login Name and their e-mail address.
Login names can contain letters (a-z, A-Z), numbers (0-9), dashes (-), underscores (_), periods (.) and at signs (@).

2. Optionally, assign Groups memberships.

3. Click the **Create** button.




# Users
## Install and connect to the ownCloud server desktop client

#### System Requirements
- Windows 7+

- Mac OS X 10.10+ (64-bit only)

- CentOS 7.6+ & 8

- Debian 9.0 & 10

- Fedora 30 & 31 & 32

- Ubuntu 18.04 & 19.04 & 19.10 & 20.04

- openSUSE Leap 15.0 & 15.1 & 15.2

With the ownCloud Desktop Sync client, your files are always automatically synchronized between your ownCloud server and local PC. The client enables you to:

- Specify one or more directories on your computer that you want to synchronize to the ownCloud server.

- Always have the latest files synchronized, wherever they are located.

### Mac OS X and Windows

1. Download the latest version of the ownCloud Desktop Synchronization Client from the [ownCloud download page](https://owncloud.com/get-started/#desktop-clients "ownCloud download page").

2. Double-click the program to launch the installation and open the **ownCloud Connection Wizard**.

3. On **Setup ownCloud Server**, enter the URL of your ownCloud server.

4. On **Enter user credentials**, enter your login information.

5. On **Setup local folder options**, select an option to sync all of your files on the ownCloud server or to select individual folders.

6. Optionally, change the default local sync folder from ownCloud in your home directory.

7. Click **Connect**.
The client will attempt to connect to your ownCloud server. When it is successful, two buttons are available:

- One to connect to your ownCloud Web GUI.

- One to open your local folder.

The client will also start synchronizing your files.

### Linux

1. Follow the instructions on the [ownCloud download page](https://owncloud.com/get-started/#desktop-clients "ownCloud download page") to add the appropriate repository for your Linux distribution.
2. Install the signing key.
3. Use your package managers to install the ownCloud Desktop Sync client.

Linux users must also have a password manager enabled, such as [GNOME Keyring](https://wiki.gnome.org/Projects/GnomeKeyring/ "GNOME Keyring") or [KWallet](https://utils.kde.org/projects/kwalletmanager/ "KWallet"), so that the sync client can log in automatically.


## Install and Connect to the ownCloud server mobile app

### iOs
Accessing your files on your ownCloud server using the web interface is easy and convenient, as you can use any web browser on any operating system without installing special client software. However, the ownCloud iOS app offers some advantages over the web interface:
- A simplified interface that fits nicely on an iPhone or iPad.

- Automatic synchronization of your files.

- The ability to share files with other ownCloud users.

- The ability to easily upload files from your device to ownCloud.

- Optional PIN for stronger security.

1. Open Safari, or any web browser, and point it to your ownCloud server. 
2. Log in and look on your Personal page for a link to the ownCloud app on iTunes.

	When you install the ownCloud app and open it, you are prompted for your ownCloud server URL and login information. When it connects, the app opens to your Files page.
	
### Android
Accessing your files on your ownCloud server using the Web interface is easy and convenient, as you can use any web browser on any operating system without installing special client software. However, the ownCloud Android app offers some advantages over the web interface:

- A simplified interface that fits nicely on a tablet or smartphone.

- Automatic synchronization of your files.

- Share files with other ownCloud users and groups, and create multiple public share links.

- Upload of photos and videos recorded on your Android device.

- Easily add files from your device to ownCloud.

- Two-factor authentication.

1.  Log into your ownCloud server from your Android device using a web browser such as Chrome, Firefox, or Dolphin.
2. The first time you log into a new ownCloud account, you’ll see a screen with a download link to the ownCloud Android App in the Google Play Store.
3. Open the ownCloud Android app.
4. On the configuration screen, enter your server URL, login name, password, and then click the **Connect** button. 



