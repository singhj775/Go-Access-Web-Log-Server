# Go-Access-Web-Log-Server
How To Install and Use GoAccess Web Log Analyzer on Ubuntu 20.04

Prerequisites

For this tutorial, you’ll need the following:

    One Ubuntu 20.04 server. You can set it by following this initial server setup for Ubuntu 20.04 tutorial, including a non-root user with sudo privileges and a firewall.

    Apache installed by following How To Install Apache on Ubuntu 20.04.


you need an Internet connection to Perform this action


**STEP 1:******************************************************************************


sudo apt update
sudo apt full-upgrade
sudo apt install libncursesw5-dev libgeoip-dev libtokyocabinet-dev build-essential

You install the following dependencies:

    build-essential: installs many packages, which includes gcc compilers for C, C+, and other programming languages, and make for building the GoAccess makefile.
    libncursesw5-dev: installs the ncurses library that GoAccess uses for its command-line interface.
    libgeoip-dev: includes the necessary files for the GeoIP library.
    libtokyocabinet-dev: provides database dependencies for higher performance.

wget http://tar.goaccess.io/goaccess-1.4.tar.gz
tar -xzvf goaccess-1.4.tar.gz

cd goaccess-1.4/

./configure --enable-utf8 --enable-geoip=legacy


You’ll receive output similar to the following:

Output
. . .
Your build configuration:

  Prefix         : /usr/local
  Package        : goaccess
  Version        : 1.4
  Compiler flags :  -pthread
  Linker flags   : -lnsl -lncursesw -lGeoIP -lpthread
  UTF-8 support  : yes
  Dynamic buffer : no
  Geolocation    : GeoIP Legacy
  Storage method : In-Memory with On-Disk Persitance Storage
  TLS/SSL        : no
  Bugs           : hello@goaccess.io

Run the make command to build the makefile required for installing GoAccess:

    make

 

Finally, install GoAccess using the previously created makefile to the system:

    sudo make install

 

Ensure that the program was installed successfully by running:

    goaccess --version

 

You will receive the following output:

Output
GoAccess - 1.4.
For more details visit: http://goaccess.io
Copyright (C) 2009-2020 by Gerardo Orellana

Build configure arguments:
  --enable-utf8
  --enable-geoip=legacy
  
  **********STEP 2:********************************************************************************************
  
  The configuration file may be located at ~/.goaccessrc or %sysconfdir%/goaccess.conf where %sysconfdir% is either /etc/, /usr/etc/, or /usr/local/etc/. To find out where the config file is located on your server, run the following command:

    goaccess --dcf

 Sample output
/etc/goaccess/goaccess.conf

Edit this config file using nano:

    sudo nano /etc/goaccess/goaccess.conf
Many of the lines in the file are commented out. To enable an option, remove the first # character in front of it. Let’s enable the time-format setting for Apache first. This setting specifies the log-format time and allows GoAccess to parse any plain-text Apache log files that meet the supported formatting criteria.
/etc/goaccess/goaccess.conf

# The following time format works with any of the
# Apache/NGINX's log formats below.
#
time-format %H:%M:%S

 

Next, you’ll uncomment the Apache date-format setting that specifies the log-format date:
/etc/goaccess/goaccess.conf

# The following date format works with any of the
# Apache/NGINX's log formats below.
#
date-format %d/%b/%Y

 

Finally, uncomment the log-format setting. Several lines change this setting and the exact one to uncomment depends on the way your web server is set up. If you have a non-virtual hosts setup, uncomment the following log-format line:
/etc/goaccess/goaccess.conf

# NCSA Combined Log Format
log-format %h %^[%d:%t %^] "%r" %s %b "%R" "%u"

 

Otherwise, if you have virtual hosts set up, uncomment the following line instead:
/etc/goaccess/goaccess.conf

# NCSA Combined Log Format with Virtual Host
log-format %v:%^ %h %^[%d:%t %^] "%r" %s %b "%R" "%u"

 

At this point, you can save the file and exit the editor. You are now ready to run the GoAccess program and analyze some Apache plain-text log files.



****STEP 3:********************************************************************************
(A) sudo goaccess /var/log/apache2/access.log


Below the top panel, you will find all the available modules which provide more details on the aforementioned metrics and other data points supported by GoAccess. To navigate the interface, use the following keyboard shortcuts:

    TAB to move forward through the available modules and SHIFT+TAB to move backwards.
    F5 to refresh the dashboard.
    g to move to the top of the dashboard screen and G to move to the last item in the dashboard.
    o or ENTER to expand the selected module.
    j and k to scroll down and up within the active module.
    s to display the sort options for the active module.
    / to search across all modules and n to move to the next match.
    0-9 and SHIFT+0 to quickly activate the respective numbered module.
    ? to view the quick help dialog.
    q to quit the program.


**********************FINAL**********************

To output the report as static HTML, specify an HTML file as the argument to the -o flag. This flag also accepts filenames that end in .json or .csv.

    sudo goaccess /var/log/apache2/access.log -o stats.html
    goaccess /var/log/apache2/nextcloud-access.log -o /var/www/html/report.html --log-format=COMBINED --real-time-html
    goaccess /var/log/apache2/access.log -o /var/www/html/report.html --log-format=COMBINED --real-time-html

####### cHANGE THE ADDRESS LOCATION AS PER YOUR NEED
