# LoRaLAN

The point of this project is to keep LoRa sensors local and off the cloud. Multiple LoRa based sensors that are built on a Raspberry Pi Pico platform using Micropython. Sensors include: A gate sensor, temperature probe, a weather station and moisture sensor. One base station (Bridge) that forwards all the sensor packets to the local network. There is no need for a cloud based account, a subscription, or a path through someone else's server. If the internet is down, the sensors still work as long as local network stays up.

The sensor nodes send RadioHead-style packets on 915 MHz(US) to one bridge and can be configured for other countries required frequencies. The bridge is the only node that answers with an ACK. From there the packet goes over Ethernet via web socket to Node-RED or other data capturing location. 

## How a packet moves

A sensor sends packets when readings change, then a slow heartbeat is used for feedback. So, a quiet node is not a dead sensor. It does not flood the LoRa spectrum with packets and allows ample bandwidth.

Another node can rebroadcast a packet received by an adjacent node one time up to two hops. It keeps the original sender and packet id. The bridge keeps the first copy and drops the repeats. The ACK comes only from the bridge (address 2). One of those ACKs can be repeated so a node farther out still hears it. This makes the LoRa sensor network robust enough that distant nodes can still be heard. 

No routing table is needed as the packets are forwarded automatically. This is a flood, not a routed mesh.

## Nodes

- [Lora to Ethernet Bridge](https://github.com/scottlecomte/Lora-to-Ethernet-Bridge) receives the packets. It is RadioHead server address 2, and it forwards them to Node-RED over Ethernet.
- [Local LoRa Weather Station](https://github.com/scottlecomte/Local-LoRa-Weather-Station) Sends environmental data (type) and rain gauge (tip bucket) 
- [Local LoRa Temperature Probe Sensor](https://github.com/scottlecomte/Local-LoRa-Temperature-Probe-Sensor) Used to monitor temperature, sends data in degrees F.
- [Local LoRa Gate Sensor](https://github.com/scottlecomte/Local-LoRa-Gate-Sensor) A simple reed switch configuration used to monitor open/closed state
- Moisture sensor repo to come - monitors the presence of moisture

Each of those repos is its own firmware. This one is only how they fit together.

Pin maps and packet shapes live in the node repos. Board design files, enclosures, and the Node-RED flows stay off this page.
