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
NMEA instruments onboard (and multplexed through kplex), it also

### Python Requirements 

The sfh script is written in python and expects python 3. It uses a number
of standard libraries (socket, select, datetime, etc.) and also requires the
following extra libraries:

  * pynmea2 (See [http://github.com/Knio/pynmea2](http://github.com/Knio/pynmea2))

  * pycrypto (See [https://www.dlitz.net/software/pycrypto/](https://www.dlitz.net/software/pycrypto/))

Both should be easily installed with pip, as in:

```
pip install pynmea2
pip install pycrypto
```

### Installation

Install the script. 

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

### FloatHub Functionality which is **NOT** supported

The main capability that a real FloatHub device provides that is not easily
replicable purely from NMEA data is voltage monitoring. This includes both
reporting of battery and charger levels as well as monitoring acitivity on
pumps. 