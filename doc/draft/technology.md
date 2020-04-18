# Technology (incomplete)

The overall goal of this project is to increase the efficiency of the road network. As such, there isn't a very specific goal or outcome for this project, as we would like to be able to pivot based on our findings, valuing the goal over the final software artefact. However, too few constraints is a surefire road to failure so we will focus exclusively on achieving the goal by enabling vehicles to communicate with each other. This allows us to explore different avenues and compare their impact but re-use a lot of the underlying technology and operate in the same technical space.

The introduction gives you a flavour of what could be possible, here we look at how to break that down into broad groups of technical capabilities.

## Scope

- If possible, this project will avoid having to invent any novel hardware or low level networking technologies to achieve its aim.
- This project assumes that a useful solution is at least analogous to putting a moderately powerful Linux desktop in a vehicle with a semi-reliable internet connection. In practice special hardware or a real time kernel with redundancy may be needed for safety, we hope this is a mostly separate concern that can be dealt with separately.
- In the early stages, everything will be focused on providing software and will simulate any hardware aspects (like vehicles driving along a road, for instance).

## Vehicle to vehicle communication

Vehicles need to be able to communicate directly in a reliable fashion and in near real time. Failure to receive timely, valid information must be distinguishable from "normal" operation and relevant protocols must be able to take this information into account and gracefully degrade.

### Hardware

For junction negotiation and object detection, it's assumed that hardware exists, or could be built, that would allow vehicles within 100m of each other to open a direct channel of communication, allowing them to pass messages using TCP with very low latency. See [Long range WiFi](https://en.wikipedia.org/wiki/Long-range_Wi-Fi), [Municipal wireless network](https://en.wikipedia.org/wiki/Municipal_wireless_network), [Bluetooth](https://en.wikipedia.org/wiki/Bluetooth).

For drafting, the latency and specifics around connection handling and reliability in current wireless systems may have a significant impact on safety. It seems feasible that a more reliable and faster vehicle to vehicle communication link could be set up using a laser or something similar, see: [Optical networking - free space optical networks](https://en.wikipedia.org/wiki/Optical_networking#Free-space_optical_networks). The vehicle also needs to be able to reliably sense its own speed and the distance between it and the vehicle in front.

It's very likely that the protocols will require a vehicle to be reliably identifiable by id. For privacy reasons that id will likely want to be dynamic (change frequently). Strong encryption between the vehicles will be required.

### Uses
- [Drafting](./capabilities.md#drafting)
- [Junction negotiation](./capabilities.md#junction-negotiation)
- [Object detection](./capabilities.md#distributed-object-detection) (sharing information about an obstruction, cyclist, pedestrian, ...)


## Vehicle to server communication

Vehicles need to be able to broadcast messages via a server.

### Hardware

Various internet connectivity options are viable, possibly a combination for redundancy. Options include: Wide Area Network, Satellite internet, Municipal Wireless Network, 3/4/5G. Thankfully, this is very much a solved problem.

Strong encryption between the vehicle and server will be required.

### Uses
- [Distributed route planning](./capabilities.md#distributed-route-planning)

## Simulator

Most of the features share the following properties:
- They're complex enough that proving their correctness is difficult or impossible
- If used in real vehicles, people would die if they malfunctioned

As such a serious investment needs to be made in simulating the environment, adding an important method of testing.
