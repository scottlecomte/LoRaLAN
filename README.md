# LoRaLAN

The point of this project is to keep the sensors local and off the cloud. A gate, a coolant temperature, and the weather do not need an account, a subscription, or a path through someone else's server. If the internet is down, the radios and the house network still work.

LoRaWAN was the wrong shape for that. These nodes send RadioHead-style packets on 915 MHz to one bridge. The bridge is the only node that answers with an ACK. From there the packet goes over Ethernet into Node-RED on the LAN.

## How a packet moves

A sensor sends when something changes, then a slow heartbeat so a quiet node is not a dead one. It does not sit on a fast timer.

Another node can rebroadcast a packet it did not send, once, up to two hops. It keeps the original sender and packet id. The bridge keeps the first copy and drops the repeats. The ACK comes only from the bridge (address 2). One of those ACKs can be repeated so a node farther out still hears it.

This is a flood, not a routed mesh. There is no route table.

## Nodes

- [Lora to Ethernet Bridge](https://github.com/scottlecomte/Lora-to-Ethernet-Bridge) receives the packets. It is RadioHead server address 2, and it forwards them to Node-RED over Ethernet.
- [Local LoRa Weather Station](https://github.com/scottlecomte/Local-LoRa-Weather-Station)
- [Local LoRa Temperature Probe Sensor](https://github.com/scottlecomte/Local-LoRa-Temperature-Probe-Sensor)
- [Local LoRa Gate Sensor](https://github.com/scottlecomte/Local-LoRa-Gate-Sensor)

Each of those repos is its own firmware. This one is only how they fit together.

Pin maps and packet shapes live in the node repos. Board design files, enclosures, and the Node-RED flows stay off this page.
