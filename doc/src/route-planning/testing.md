# Testing

## Test cases

### Routing

- Vehicle chooses the _optimal static route_ when no relevant information to contradict this route has been received.
- Vehicle manages to choose next best route after receiving information that the _optimal static route_ is suffering congestion.

### Withstanding attacks

- Vehicle receives incorrect information and successfully disregards it based on contradictory information that has been previously received by multiple sources.
- Vehicle receives incorrect information and successfully disregards it based on contradictory information that has been previously received by single source where we've been able to verify the correctness.
- Vehicle retrospectively disregards incorrect information after noticing it's wrong.
