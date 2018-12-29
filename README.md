# sfh
A software-only implementation of the FloatHub protocol.

The FloatHub system provides a means for monitoring marine vessels remotely.
Although there is a commercial device available to support this
functionality, much of the monitoring capability can be achieved purely
in software. 

A typical installation would require:

  * The script in this repository (sfh)

  * A small onboard computer (typically a Rasberry Pi)

  * A source of NMEA data (generally something like kplex or gpsd, see below)


This would allow for remote monitoring of location, speed, etc. With other
NMEA instruments onboard (and multplexed through kplex), it also

## NMEA Sentences Monitored

  * GGA - GPS fix and precision of fix 

  * RMC - GPS Date/Time data, speed over ground, course over ground

  * XDR - Air Temperature, Water Temperature, and Barometric Pressue

  * MDA - Temperature, Pressure, True Wind Speed and Direction

  * DBT - Water Depth

  * DPT - Water Depth

  * MTW - Water Temperature

  * VHW - Speed through Water

  * MWV - Wind Speed and Angle (Relative)

  * MWD - Wind Speed and Direction

  * HDG - Heading (True and Magnetic)

  * HDM - Heading (Magnetic)

  * HDT - Heading (True)

