# Introduction (requires proof read)

`traffic-light` is a software project, currently in vaporware mode, exploring ways to increase the efficiency of traffic on the road network.

If `traffic-light` had a stated goal it would be:

> Increase the efficiency of the road network, especially in cities, by providing vehicles with the technology to share information with each other.

Stated another way, we believe that networked vehicles offer significant advantages over independent agents following a single, static rule system (e.g. the highway code). We're also aware of the impact this may have on privacy and look to find ways to mitigate that impact or make sensible trade offs to balance efficiency with privacy.

Examples of things that become possible include: much better fuel efficiency, higher top speed, reduced need to wait at traffic lights, superior route planning and better object detection.

A lot of the technology is aimed at fully autonomous vehicles (self driving cars), but some of it could be similarly beneficial with human drivers.

## Speed and efficiency

If two vehicles could reliably communicate in near real time, it would be possible for vehicles to drive very close together. Currently this isn't safe because limitations in human reaction time mean that if the vehicle in front braked abruptly, the following vehicle would crash into it. The act of driving very close to the vehicle in front is sometimes known as drafting and it's beneficial because the vehicle(s) behind the lead vehicle experience drastically reduced air resistance (drag), which is one of the major forces that a vehicle has to overcome. Overcoming this force uses significant amounts of fuel, thus limiting the vehicle's range. Another possible option would be to build powerful vehicles to take on the role of lead vehicle, allowing smaller, cheaper, more efficient vehicles to achieve a very high top speed in practice.

## Junction negotiation

If a group of vehicles approaching a junction could communicate with each other, and a suitable protocol existed, it would be possible for them to move through a junction without having to stop and give way to the other vehicles. The vehicles could instead negotiate speed and position such that they could safely interleave as they passed through the junction. There are many reasons this would be difficult in practice: taking into account sudden communication failure; how the human in the vehicle feels about it; and non-autonomous, non-networked vehicles to name but a few. However, in constrained environments this may significantly increase throughput and therefore is a worthwhile topic of research.

## Environmental awareness

It stands to reason that if each vehicle, or the vehicle's driver, had more environmental information, it could make better decisions.

### Route planning

If each vehicle broadcast information about itself, its intentions, and what it could see around it, the recipients of that information could model their environment and take a more efficient -- notice the careful avoidance of the words "most efficient" -- route to their destinations. If an emergency occurred, vehicles could efficiently route their way around it, reducing their disruption and allowing emergency services to arrive on scene faster. This comes with a few inherent privacy trade offs that will be discussed in more detail in the relevant sections.

### Object detection

Two sensors are better than one. If vehicles can collaborate on sensing their environment, it's likely they will be able to do a superior job of protecting cyclists and pedestrians, avoiding obstacles and safely navigating their environment.

## The privacy trade off

If we broadcast lots of information about ourselves and our intentions, then it would be fairly trivial for others to map out our movement habits and grossly invade our privacy, plan when best to burgle our houses, find out who we're meeting, etc. If we passed all this information to and from a single entity, be it government or private organisation, we would endow that entity with significant power: they could charge us money for better information, give us false information on purpose, put us to the front or back of queues based on compliance with a set of rules (legal or commercial), and so the list goes on. It's certainly not the goal of `traffic-light` to propose the right balance to these trade offs, or get deep into the philosophy of these issues, but we acknowledge their importance and existence and will take them into account when designing our systems. In our opinion we think the best system will maximise efficiency and useful information transfer whilst minimising exposure of private information and single points of control. Where it's impossible to work around the inherent trade off, we will take what we believe to be a desirable middle ground erring on the side of increasing privacy and reducing single points of control at the expense of efficiency. We aren't attempting to make this decision for society, rather we're demonstrating one way this technology can work, allowing the free market and legal systems to take it from there.
