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

The sfh script is written in python and expects python 2.7.  It will
probably run under python 3, but re-connections to intermittent NMEA sources
do not appear to be as reliable.  It uses a number of standard libraries
(socket, select, datetime, etc.) and also requires the following external
libraries:

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

On Windows it is a little more complicated, but you should be able to find
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
found [here](http://www.stripydog.com/kplex/configuration.html#tcp).

There are, of course, many other ways to get NMEA data from a TCP source,
including [gpsd](http://catb.org/gpsd/). A typical GPSD setup will serve out
position (GPS) data on port 2947.

Whatever your source and setup, you need to know the hostname and port where
NMEA data is being served over TCP.  The hostname will often be 127.0.0.1
and/or localhost, but will depend on your particular configuration.

### Usage

You can run the sfh with the `--help` flag to see all it's available
options:

```
./sfh --help
usage: sfh [-h] [-v] [-n] [-s S] [-p P] [-f F] [-t P] [-i I] [-k K]

Consume NMEA over TCP and send it to a FloatHub server in the right format
(see https://floathub.com)

optional arguments:
  -h, --help        show this help message and exit
  -v, --verbose     increase output verbosity with debugging info
  -n, --noais       ignore (do not relay) AIS sentences
  -s S, --source S  hostname of nmea source (default 127.0.0.1)
  -p P, --port P    port of nmea source (default 10110)
  -f F, --fhub F    hostname of FloatHub server (default fdr.floathub.net)
  -t P, --fhport P  port of FloatHub Server (default 50003)
  -i I, --id I      FloatHub Acount ID (default factoryX)
  -k K, --key K     FloatHub Security Key (default
                    000102030405060708090A0B0C0D0E0F)


```

The most import settings are the host and port for the source of the data and
the FloatHub ID and Security Key to identify the account. So, for example, if your
kplex process is running on port 333333 on `localhost`, your FloatHub ID is
`foobar99` and your Security Key is `42424242424242424242424242424242`, you
would run sfh like this:

```
./sfh -p 33333 -s localhost -i foobar99 -k 42424242424242424242424242424242
```

The sfh script will just keep monitoring NMEA data from `localhost:33333`
and sending it off to the FloatHub system.  It is designed to be a very long
running process. If it gets disconnected, it will keep attempting to
periodically reconnect (to both the NMEA source and the FloatHub system). 
If you want to see more verbose information about what's going on, you can
the '-v' flag to get more output.


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

  * AIS - AIS Relaying (see below)

### FloatHub Functionality which is NOT supported

The main capability that a real FloatHub device provides that is not easily
replicable purely from NMEA data is voltage monitoring. This includes both
reporting of battery and charger levels as well as monitoring activity of
pumps. 

### What About AIS Data?

The script will relay AIS sentences to the FloatHub server if they are
present in the NMEA stream (just like a regular FloatHub device does). On
the FloatHub website it is possible to echo this data to AIS aggregation
sites such as 
[MarineTraffic.com](https://www.marinetraffic.com/)
or 
[AISHub.net](http://www.aishub.net)
