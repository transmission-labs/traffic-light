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
- **Initial:** the vehicle MUST accept a destination and create an initial route plan.
- **Update:** the vehicle MUST update its route plan based on information received.
- **Map:** the vehicle MUST read and use the map provided in configuration.

### Mapping
- TODO: specify the map format.

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

- MUST work in a language agnostic manner (but not necessarily platform agnostic)

### Spawning
- MUST spawn vehicles (processes).
- MUST set up and facilitate vehicle to vehicle communication (via sockets or similar).
- MUST set up and facilitate simulator to vehicle communication.
- MUST wait 5 seconds for a route to be provided by the vehicle on the telemetry socket
  - If no route is provided, terminate the vehicle process and log an error.
- MUST send a destination to the vehicle.
- MUST immediately start sending the stream of GPS coordinates representing the location of the vehicle (starting point).

### Moving

- MUST move the vehicle along the route provided by the vehicle
  - by passing in updated GPS coordinates
  - going at the speed limit of the road
  - waiting at traffic lights
  - stopping at give way junctions
  - without vehicles hitting each other
  - slowing down if the traffic becomes congested
- SHOULD
  - NOT magically accelerate to top speed when pulling away from a standstill

### Map
- MUST use a map provided in `traffic-light` format
  - TODO: map format spec
- MUST pass the same map to all vehicles


### Test cases

_work in progress_

### Visualisation

_work in progress_

## Map

The plan is to use [openstreetmap.org](https://www.openstreetmap.org/) [exports](https://download.openstreetmap.fr/extracts/) to build the maps used by the simulators. At first glance it appears it would be beneficial to create our own mapping format that allows for clear and performant use of the data, rather than parsing osm XML directly. This way we can create separate utility that converts osm XML to our format and prevent the vehicle code or simulator logic becoming complex and coupled.

The map has two interpretations: a physical interpretation which corresponds to reality and is most amenable to drawing; a logical interpretation that allows for clear code around decision making. For example, the logical interpretation is a graph, where nodes represent junctions and edges represent road segments, that makes it simple to annotate concepts such as "no left turn between edge-a and edge-b". It's possible that all the physical information can be added to the logical one and we have a single format.

### Logical representation

The below is represented in EDN because it's concise and expressive. You could easily imagine it as JSON, a struct, etc.

```clojure
;; Simple example map
;; Drive on right hand side

;;      | 2 |
;;      |   |
;;->->--  | |
;;__3___    |
;;      | 1 |
;;      |   |
;;      | | |

;; Node
{:type :give-way-junction
 :edges [1 2 3]
 :relations [{:from 1 :to 3 :relation :no-turn :description "No left turn"}
             {:from 2 :to 3 :relation :no-turn :description "No right turn"}
             {:from 3 :to 1 :relation :give-way}
             {:from 3 :to 2 :relation :give-way}
             {:from 1 :to 2 :relation :flow}
             {:from 2 :to 1 :relation :flow}]}

;; Edge examples
{:id 1
 :way-name "St Nicholas Way"
 :segment-number 1
 :direction {:type :two-way}
 :speed-limit {:value 50 :units :mph}
 :points [{:lat 50.0 :lon 0.0} {:lat 50.1 :lon 0.0} {:lat 50.2 :lon 0.0}]}

{:id 2
 :way-name "St Nicholas Way"
 :segment-number 2
 :direction {:type :two-way}
 :speed-limit {:value 50 :units :mph}
 :points [{:lat 50.3 :lon 0.0} {:lat 50.4 :lon 0.0} {:lat 50.5 :lon 0.0}]}

{:id 3
 :way-name "Side St Road"
 :segment-number 1
 :direction {:type :one-way :from {:lat 50.2 :lon -0.2} :to {:lat 1.23 :lon 0.0}}
 :speed-limit {:value 30 :units :mph}
 :points [{:lat 50.2 :lon 0.0} {:lat 50.2 :lon -0.1} {:lat 50.2 :lon -0.2}]}
```


## Limits

| Type               | Value         | Description |
| ------------------ | ------------- | ----------- |
| Time to first byte | ?             | From a vehicle feasibly being able to connect to another vehicle, how long will it take to start transferring information, and what's the distribution. |
| Max bytes at 70mph | ?             | How many bytes can two vehicles transfer (estimated) if they travel in opposite directions at 70mph |


[^1]: The details of this could be non trivial in practice, depending on the networking technology used. If it were similar to WiFi it's not clear which vehicle would host the LAN and which would connect, and if you ended up with simultaneous connections where both vehicles were host and guest at the same time, this would have to be resolved. For the purposes of the prototype we're glossing over this detail for now in order to learn about how the routing works. Two entities coming into contact and establishing a unique connection is very likely a solved or solvable problem.

[^2]: NB: the vechile can be "driven" by simply pushing a stream of GPS coordinates in, the traffic light routing module won't choose to move or not move.

[^3]: This will already be heavily processed from the raw sensor input (radar + lidar + <...> &rarr; other vehicles' positions).
