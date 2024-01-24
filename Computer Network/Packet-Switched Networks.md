# Packet-Switched Networks

## Table of Contents
- [Delays](#delays)
  - [Processing Delay](#processing-delay)
  - [Queueing Delay](#queueing-delay)
  - [Transmission Delay](#transmission-delay)
  - [Propagation Delay](#propagation-delay)


## [Packet Switching](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Computer-Network.md#packet-switching)

## Delays

<div align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/figure1.16-the-nodal-delay-at-router-a.png" alt="Figure 1.16 - The nodal delay at router A" width="80%" />
  <p><em>Computer Network: Top-down Approach - Figure 1.15</em></p>
  <br />
  <i>Total Nodal Delay = d<sub>processing</sub> + d<sub>queueing</sub> + d<sub>transmission</sub> + d<sub>propagation</sub></i>
</div>

### Processing Delay
- The time required to check the packet's header and determine where to direct the packet (the queue for the target outbound Link).
  - It could also include the time to check for bit-level errors in the packet that may have occured in transmitting the packet's bits from the upstream node to this node.
- Microseconds or less.
### Queueing Delay
- The time that a packet waits in the output queue/buffer for its turn to be transmitted onto the outbound Link.
  - If there are no preceding packets in the queue, the queueing delay will be 0.
- Microseconds to milliseconds.
### Transmission Delay
- The time required to push/transmit all of the packet's bits onto the outbound Link.
- <i>d<sub>transmission</sub> = L/R</i>
  - L = packet size in bits
  - R = transmission rate (bits per second)
  - Ex: 0.016Mb / 10Mbps = 0.0016s
- Microseconds to milliseconds.
### Propagation Delay
- The time required to actually propagate a bit from the current node to the next node.
  - Depends on the physical medium of the Link (Ex: fiber optics, twisted-pair copper wire).
- <i>d<sub>propagation</sub> = distance / propagation speed</i>
  - The propagation speed is between 2 * 10<sup>8</sup>m/s and 3 * 10<sup>8</sup>m/s (approx. speed of light).
  - Ex: 5,000km / 300,000km/s
