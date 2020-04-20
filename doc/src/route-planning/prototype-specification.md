# Specification (prototype)

Whilst we've tried to make the documentation engaging and accessible, this is most likely "the boring part", unless you're especially interested in the minutiae and have a very strong technical foundation, you would be forgiven for skipping this.

This is the _prototype_ specification, the emphasis will be more on learning and implementation speed and less on correctness and safety.

The specification is broken into two broad parts: the code that will actually run in a vehicle, a test rig (simulator) that can run it to see how it behaves.

## Vehicle code

If you get lost in a fog of detail, all of the following simply enables:
- Vehicles must be able to talk to each other.
- This code must be run through a simulator and in a real vehicle whilst using the exact same code.
- A vehicle needs to receive information from other vehicles and update its route based on that information:
  - so it has to know where it is right now.
- A vehicle needs to provide other vehicles with the information it's seen.

### Vehicle to vehicle connection

- **Scanning:** the vehicle MUST constantly scan for other vehicles and attempt to open a connection with them[^1].
  - Each vehicle MUST do all network communication via a socket/module (details provided in configuration) (TODO: details of type of socket/module etc).
  - Each vehicle MUST be able to receive information about new connections (new vehicles that have be "found" by scanning) on the socket. Vehicles (connections) identified by UUIDv4.
  - Each vehicle MUST use this socket/module to _request_ data transfer and listen to requests from another vehicle (open/negotiate connection, receive/send stream of bytes).

### Data transfer
- **Requesting info:** the vehicle MUST request information from connections that become known to it.
  - The request CAN provide information about where the requesting vehicle is heading (to be used for prioritisation).
- **Sending:** the vehicle MUST be able to receive requests and send _prioritised_ information to other vehicles.
  - The information MUST be prioritised by location and recipient heading (if provided).
- **Format:** TODO: specify format

### Route planning
- **Initial**: the vehicle MUST accept a destination and create an initial route plan.
- **Update**: the vehicle MUST update its route plan based on information received.

### Telemetry
- The vehicle MUST push telemetry to a socket/module provided in configuration (TODO: details to follow).
  - The vehicle MUST send its route when it first calculates it.
  - The vehicle MUST send the new route if it updates the route.

### Sensors
- Each vehicle MUST be able to receive sensor information by listening to a socket/module (details provided in configuration) (TODO: details of type of socket/module etc)
  - **GPS**: the vehicle MUST accept a stream of GPS coordinates.[^2]
  - **Immediate surroundings information**: the vehicle SHOULD accept a stream of information about its surroundings[^3].

### Trust
- **Incorrect information:** the vehicle SHOULD be able to assign a trust score to information it receives and incorporate that into its route planning.

## The simulator

_work in progress_

### Test cases

_work in progress_

### Visualisation

_work in progress_

## Limits

| Type               | Value         | Description |
| ------------------ | ------------- | ----------- |
| Time to first byte | ?             | From a vehicle feasibly being able to connect to another vehicle, how long will it take to start transferring information, and what's the distribution. |
| Max bytes at 70mph | ?             | How many bytes can two vehicles transfer (estimated) if they travel in opposite directions at 70mph |


[^1]: The details of this could be non trivial in practice, depending on the networking technology used. If it were similar to WiFi it's not clear which vehicle would host the LAN and which would connect, and if you ended up with simultaneous connections where both vehicles were host and guest at the same time, this would have to be resolved. For the purposes of the prototype we're glossing over this detail for now in order to learn about how the routing works. Two entities coming into contact and establishing a unique connection is very likely a solved or solvable problem.

[^2]: NB: the vechile can be "driven" by simply pushing a stream of GPS coordinates in, the traffic light routing module won't choose to move or not move.

[^3]: This will already be heavily processed from the raw sensor input (radar + lidar + <...> &rarr; other vehicles' positions).
