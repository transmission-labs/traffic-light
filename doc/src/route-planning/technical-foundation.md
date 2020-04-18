# Technical foundation

## System

The system we're designing is a peer to peer information sharing system where vehicles can send each other information when they're physically close enough to establish a connection. This information will then be used in conjunction with a detailed map to provide routing information.

## Hardware

The majority of this project is going to simulate the lower level parts of the stack, however, we must ensure that we're embarking on something plausible. What follows is a short discussion of the required technological foundation for this approach. Much later, we would like to test these systems in real world conditions.

### Vehicle setup

The vehicle will need the following components:

- Onboard computer that's powerful enough to store and process the necessary information. Specs currently unknown, but a rough idea is: 4 core CPU ~ 3GHz, 16 GB RAM, 512GB disk, cost approx £300 - £700 ($375 - $875).
- Wireless peer to peer networking hardware: aerial, wireless network card, [similar to wifi](https://en.wikipedia.org/wiki/Vehicular_ad-hoc_network#Technology).
- Onboard map, probably provided by [openstreetmap.org](https://www.openstreetmap.org/)
- Internet connection to provide map updates and extra information (possibly: weather, extra sources of information on accidents and traffic reports, ...).
