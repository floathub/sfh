# sfh
A software-only implementation of the FloatHub protocol.

The FloatHub system provides a means for monitoring marine vessels remotely.
Although there is a commercial device available to support this
functionality, much of the monitoring capability can be achieved purely
in software. 

A typical installation of this code would require:

  * The phython script in this repository (sfh)

  * A small onboard computer (typically a Rasberry Pi)

  * A TCP source of NMEA data (generally something like [kplex](http://www.stripydog.com/kplex/) or [gpsd](http://catb.org/gpsd/), see below)

This would allow for remote monitoring of location, speed, etc. With other
NMEA instruments onboard (and multplexed through kplex), it would also allow
for remote monitoring of wind, water, barometric pressure, etc.

### Python Requirements 

The sfh script is written in python and expects python 3. It uses a number
of standard libraries (socket, select, datetime, etc.) and also requires the
following external libraries:

  * pynmea2 (See [http://github.com/Knio/pynmea2](http://github.com/Knio/pynmea2))

  * pycrypto (See [https://www.dlitz.net/software/pycrypto/](https://www.dlitz.net/software/pycrypto/))

Both should be easily installed with pip, as in:

```
pip install pynmea2
pip install pycrypto
```

### Installation

You can grab the sfh script by clicking on the green "Clone or download"
button on the [github page](https://github.com/floathub/sfh).  Once the sfh
file is copied to a place where you can execute it (probably on a Pi or
other small, onboard computer), you need to make sure it is set as
exectuable.  On Linux/Mac/etc., you should be able to do this by typing:

```
chmod a+x ./sfh
```

On Windows it is a little more compliacted, but you should be able to find
guidance [here](https://docs.python.org/3/faq/windows.html). 


### FloatHub Credentials

You'll also need a **FloatHub ID** and **Security Key**. These are free and
require no payment or personal information other than an email address.
These account credentails are required to identify which incoming data
belongs to which vessel, and also to help encrypt the data before it is
sent. If you don't already have a FloatHub account, you can get one here:

  https://floathub.com/signup

Once you have an account, you can copy your **FloatHub ID** 
and **Security Key** by going to the account page:

  https://floathub.com/account

and clicking on the "Show FloatHub Device Settings" button. 

### NMEA Data Source

Finally, you'll need a TCP source of NNMEA data. This is typically from
something like kplex, which is a handy "broker" of NMEA data often used
with onboard navigation systems built around Rasberry Pi Computers. 

Much more information on kplex can be found at its [homepage](http://www.stripydog.com/kplex/).
In particular, configuring it to serve NMEA data as a TCP server can be
found [here] (http://www.stripydog.com/kplex/configuration.html#tcp).

There are, of course, many other ways to get NMEA data from a TCP source,
including [gpsd](http://catb.org/gpsd/). A typical GPSD setup will serve out
position (GPS) data on port 2947.

Whatever your source and setup, you need to know the hostname and port
where NMEA data is being served over TCP. The hostname will often be
127.0.0.1 and/or localhost, but will depend on your particular setup and
configuration.

### Usage



### NMEA Sentences Monitored

  * GGA - GPS Latitude, Longitude, and Precision

  * RMC - GPS Date/Time, Speed Over Ground, Course Over Ground

  * XDR - Air Temperature, Water Temperature, and Barometric Pressue

  * MDA - Temperature, Pressure, True Wind Speed and Direction

  * DBT - Water Depth

  * DPT - Water Depth

  * MTW - Water Temperature

  * VHW - Speed Through Water

  * MWV - Wind Speed and Angle (Relative)

  * MWD - Wind Speed and Direction

  * HDG - Heading (True and Magnetic)

  * HDM - Heading (Magnetic)

  * HDT - Heading (True)

### FloatHub Functionality which is NOT supported

The main capability that a real FloatHub device provides that is not easily
replicable purely from NMEA data is voltage monitoring. This includes both
reporting of battery and charger levels as well as monitoring acitivity of
pumps. 