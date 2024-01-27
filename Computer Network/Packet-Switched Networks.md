# Packet-Switched Networks

## Table of Contents
- [Nodal Delay](#nodal-delay)
  - [Processing Delay](#processing-delay)
  - [Queueing Delay](#queueing-delay)
    - [Queueing Delay and Packet Loss](#queueing-delay-and-packet-loss)
      - [Traffic Intensity](#traffic-intensity)
  - [Transmission Delay](#transmission-delay)
  - [Propagation Delay](#propagation-delay)
- [End-to-End Delay](#end-to-end-delay)

## [Packet Switching](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Computer-Network.md#packet-switching)

## Nodal Delay
- Delay at a single router.

<div align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/figure1.16-the-nodal-delay-at-router-a.png" alt="Figure 1.16 - The nodal delay at router A" width="80%" />
  <p><em>Computer Network: Top-down Approach - Figure 1.15</em></p>
  <br />
  <i>Total Nodal Delay = d<sub>processing</sub> + d<sub>queueing</sub> + d<sub>transmission</sub> + d<sub>propagation</sub></i>
</div>

### Processing Delay
- The time required to check the packet's header and determine where to direct the packet (the queue for the target outbound link).
  - It could also include the time to check for bit-level errors in the packet that may have occured in transmitting the packet's bits from the upstream node to this node.
- Microseconds or less.
### Queueing Delay
- The time that a packet waits in the output queue/buffer for its turn to be transmitted onto the outbound link.
  - If there are no preceding packets in the queue, the queueing delay will be 0.
- Unlike the other delays, the queueing delay can vary from packet to packet.
- Microseconds to milliseconds.
#### Queueing Delay and Packet Loss
- The significance of the queueing delay depends on 1) the rate at which traffic arrives at the queue, 2) the transmission rate of the link, and 3) whether the traffic arrives periodically or in bursts.
##### Traffic Intensity
- Traffic Intensity is a ratio used to estimate the extent of the queueing delay.
- <i>Traffic Intensity = L*a / R</i>
  - L = packet size in bits
  - a = rate at which packets arrive at the output queue (packets per second)
  - L*a = rate at which bits arrive at the output queue (bits per second)
  - R = transmission rate (bits per second)
- If Traffic Intensity > 1, the queue is receiving more bits than it is able to transmit bits to the outbound link.
  - As the Traffic Intensity gets closer to 1, the average queue length gets larger and larger.
  - Golden rule - Traffic Intensity should not be greater than 1.
- If Traffic Intensity <= 1, the queueing delay is greatly affected on whether the traffic arrives periodically or in bursts.
  - If packets arrive periodically, there will be minimal queueing delay.
  - If packets arrive in bursts, the average queueing delay will be significant.
    - The first packet will have no queueing delay but the last packet will have a queueing delay of _(n - 1)L/R_ seconds.
##### Packet Loss
- Since an output queue has a finite capacity, when a packet arrives at a full queue, the packet or one of the packet in the queue will be dropped (depending on the router).
- At the end-system, a lost packet will look like it having been transmitted to the network core but never showing up at the destination.
- Try `ping <hostname or ip address>` to test network latency and packet loss.
### Transmission Delay
- The time required to push/transmit all of the packet's bits onto the outbound Link.
- <i>d<sub>transmission</sub> = L/R</i>
  - L = packet size in bits
  - R = transmission rate (bits per second)
    - _i.e. the rate at which bits are pushed out of the output queue._
  - Ex: 0.016Mb / 10Mbps = 0.0016s
- Microseconds to milliseconds.
### Propagation Delay
- The time required to actually propagate a bit from the current node to the next node.
  - Depends on the physical medium of the Link (Ex: fiber optics, twisted-pair copper wire).
- <i>d<sub>propagation</sub> = distance / propagation speed</i>
  - The propagation speed is between 2 * 10<sup>8</sup>m/s and 3 * 10<sup>8</sup>m/s (approx. speed of light).
  - Ex: 5,000km / 300,000km/s

## End-to-End Delay
- Total delay from source to destination.
- <i>d<sub>end-end</sub> = N(d<sub>processing</sub> + d<sub>transmission</sub> + d<sub>propagation</sub></i>
  - Assuming that:
    - the network is uncongested (queueing delays are negligible).
    - <i>d<sub>processing</sub></i> is the same at each router.
    - transmission rate out of the source host and routers are the same (R bits/sec).
    - <i>d<sub>propagation</sub></i> is the same for each link.
- Try `traceroute <hostname or ip address>`.
  - Sends multiple special packets towards the destination (does this process 3 times).
  - When a router receives a special packet, it sends back a short message about itself to the source.
  - The round-trip delays (in ms) include the nodal delay.
    - Due to the queueing delay varying, the nth router's delay can be longer than that of the n+1 router.
    - A big increase in delay between routers indicates a long link (Ex: transatlantic link).
  - `*` indicates that the source received fewer than three messages from the router due to packet loss in the network.
  - RFC 1393.

## End System Delay
- An end system transmitting a packet into a shared medium (Ex: WiFi, cable modem) may experience purposeful delay.
- Media packetization delay (Ex: VoIP).


