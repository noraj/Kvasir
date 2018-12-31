Kvasir
======

![](https://img.shields.io/github/issues/KvasirSecurity/Kvasir.svg)
![](https://img.shields.io/github/forks/KvasirSecurity/Kvasir.svg)
![](https://img.shields.io/github/stars/KvasirSecurity/Kvasir.svg)
[![Rawsec's CyberSecurity Inventory](https://inventory.rawsec.ml/img/badges/Rawsec-inventoried-FF5050_flat.svg)](https://inventory.rawsec.ml/tools.html#Kvasir)

Welcome to Kvasir! Herein these directories lay the groundwork tools for
effective data management during a Penetration Test.

Penetration tests can be data management nightmares because of the large
amounts of information that is generally obtained. Vulnerability scanners
return lots of actual and potential vulnerabilities to review. Port
scanners can return thousands of ports for just a few hosts. How easy is
it to share all this data with your co-workers?

That's what Kvasir is here to help you with. Here's what you'll need to get
started:

 * The latest version of web2py (http://www.web2py.com/)
 * A database (PostgreSQL known to work)
 * A network vulnerability scanner (Nexpose, Nessus and Nmap supported)
 * Additional python libraries

Kvasir is a web2py application and can be installed for each customer or
task. This design keeps data separated and from you accidentally attacking
or reviewing other customers.

This tool was developed primarily for the Cisco Systems Advanced Services
Security Posture Assessment (SPA) team. While not every methodology may not
directly align, Kvasir is something that can be molded and adapted to fit
almost any working scenario. Pull requests through Github are encouraged!


DOCUMENTATION
=============

Current documentation will be maintained on the Kvasir Github wiki
(https://github.com/KvasirSecurity/Kvasir/wiki)


NOTES
=====

Kvasir was primarily designed for use on short customer-focused engagements.
A directory 'application' for each customer would be used allowing for much
stronger data separation.

For example lets assume two customers, *Foo Widgets* and *Bar Napkins*.

Data for each customer is stored in /opt/data/$CUSTOMERNAME

Install Kvasir in each customer's directory:

 * git clone https://github.com/KvasirSecurity/Kvasir /opt/data/foowidgets/kvasir
 * git clone https://github.com/KvasirSecurity/Kvasir /opt/data/barnapkins/kvasir

Now symbolically link Kvasir to the web2py application directory:

 * ln -s /opt/data/foowidgets/kvasir $WEB2PY_HOME/applications/foowdigets
 * ln -s /opt/data/barnapkins/kvasir $WEB2PY_HOME/applications/barnapkins

Create unique databases:

 * sudo su - postgres
 * createdb -O pguser foowidgets
 * createdb -O pguser barnapkins

Copy the kvasir.yaml.sample to kvasir.yaml and change the defaults:

 * db->kvasir->uri

You're ready to go!


WEB2PY SCHEDULER TASK QUEUE
===========================

The web2py scheduler task system is used for long-running tasks such as
launching terminals, processing XML report files, etc. The scheduler can run
with the main web2py process or started from a separate terminal.

To start as part of the web2py web server process:

    cd $WEB2PY_HOME
    python web2py.py -a <recycle> -X -K foowidgets,barnapkins

To start as its own process:

    cd $WEB2PY_HOME
    python web2py.py -K foowidgets,barnapkins

Additional workers may be started by repeating the application name:

    cd $WEB2PY_HOME
    python web2py.py -K foowidgets,foowidgets,barnapkins,barnapkins


LOGGING
=======

By default the scheduler task logging level is DEBUG. This can get very
noisy on a terminal. To change this copy the logging.conf file to the web2py
home directory. Modify it as you see fit.

    cd $WEB2PY_HOME
    cp applications/$APPNAME/logging.conf .

