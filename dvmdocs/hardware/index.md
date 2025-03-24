# DVMProject Hardware Overview

DVMProject maintains several in-house designs for interfacing with physical radio devices.

## DVM-V1

![DVM-V1 Photo](/dvmdocs/media/hardware/dvm-v1.jpg)

The DVM-V1 is a derivative of the [Multi-Mode Digital Voice Modem (MMDVM)](https://github.com/g4klx/MMDVM) design originally 
created by Jon Naylor, G4KLX. The DVM-V1 modem allows for conventional analog radio devices to modulate and demodulate C4FM/4-FSK/TDMA
digital radio signals (used by P25, DMR, and NXDN). In essence, any analog radio system can be converted to a full-featured conventional
or trunked digital repeater system using nothing more than a single modem and a repeater, duplex base station, or any combination of
radio receiver and transmitter.

[Read More](dvm-v1.md)

## DVM-V24

![DVM-V24 Photo](/dvmdocs/media/hardware/dvm-v24.jpg)

The DVM-V24 is a totally new in-house hardware design intended to adapt legacy Motorola V.24 synchronous serial used by base stations and
comparators like the Quantar and AstroTAC to the [dvmhost](/dvmdocs/software/dvmhost/index.md) application.

[Read More](dvm-v24.md)