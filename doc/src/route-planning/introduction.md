# Route planning

The goal of the route planning module is to provide technology that allows all road users to find efficient routes from their starting point to their destination.

There are some constraints within which we would like to achieve the goal:

- **Open:** the system must be open so that it benefits everyone fairly and will continue to do so as it evolves.
- **Robust:** the system must be resilient to [malicious actors](https://www.wired.com/story/99-phones-fake-google-maps-traffic-jam/).
- **Distributed:** the system must not centralise trust in a single organisation or entity.
- **Powerful:** the system must provide compelling advantages over a closed or centralised system, so people are incentivised to adopt it.

## How's this different to existing technology?

There already exist technologies that do an excellent job of finding routes, and even incorporating traffic information. The [Open Source Routing Machine - OSRM](https://github.com/Project-OSRM) uses [openstreetmap.org](https://www.openstreetmap.org/) data to provide routes from A->B and [google maps](https://www.google.com/maps) similarly provides routing, which can also adapt to current traffic conditions.

As far as we know, there isn't an existing system that's open to everyone, resilient in the face of malicious actors, distributed rather than controlled by a single organisation and powerful enough to provide dynamic[^1] routing.

## Examples

The module would be successful if it could do the following things[^2]:

- Effectively use the available roads by distributing traffic amongst them evenly, taking into account road type and protecting residential streets from unacceptable levels of traffic.
- Reach full capacity of the road network before encountering traffic jams.
- Find out in real time if an accident blocked a road, and route around it.
- Clear a path for emergency vehicles without substantially increasing journey time.
- Be resilient in the face of users maliciously or accidentally sharing incorrect data.

## Adoption

The technology simply won't work without a significant number of users. Part of the project is to understand how many people would have to adopt the technology to make it worthwhile. A possible path to adoption is to encourage an organisation that could provide the critical mass to adopt it. One could imagine that if every taxi and ride sharing driver in a city adopted the technology, critical mass could be achieved and the benefits would then attract all other road users to the technology.


[^1]: "Dynamic" routing is routing that adapts to a constantly evolving view of the current traffic conditions. By contrast static routing is based on facts known ahead of time, whilst this can be quite sophisticated, like including likely traffic for the time of day, it doesn't include information about what's actually happening at that point in time.

[^2]: Some of these aren't likely in the near term, but one could imagine them being possible.
