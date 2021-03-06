# cfsd-proxy-cangw
cfsd proxy CAN getway

Mapping the CAN messages to Opendlv messages

This version is for the communicate with the lynx car

messages:

The mapping info of messages can be found at CAN Mapping.md

lynx-v0.1.1.odvd is the message set for talking to CAN should only be used in these CAN getway microservice.

lynx19gw.dbc is the CAN database file which is a reference for decoding and encoding the CAN message.

lynx19gw.dbc.map is the mapping setting file for the microservice knowing how to map the messsages.

Messege senderStamps See: CAN Mapping.md

-- update:
2019-04-10 Change Data logger sending message value, setting offsets and scale. (To adapt the proxy to the datalogger format see: https://github.com/chalmersfsd/CFS17_Firmware/blob/master/C_FW/Libraries/GUI.c#L1070 )

2019-04-14 ADD Messages : KnobL and KnobR

2019-05-06 Switch the lynx-v0.1.1.odvd message set to cfsd-extended-message-set-v0.0.1.odvd, changes message names

run the microservice:

```
docker run --rm -ti --net=host --privileged chalmersfsd/cfsd-proxy-cangw-lynx:v0.1.2 --cid=111 --can=can0 --verbose
```


generate the dbc map file:

```
docker run --rm -ti -v $PWD/src/:/in -w /in chalmersrevere/dbc2odvd-amd64:v0.0.6  generateHeaderOnly.sh lynx19gw.dbc cfsd-extened-message-set-v0.0.1.odvd
```

get the code Snippet:

```
docker run --rm -ti -v $PWD/src/:/in -w /in chalmersrevere/dbc2odvd-amd64:v0.0.6 generateMappingCodeSnippet.awk lynx19gw.dbc.map
```

Setting the CAN:

```
sudo ip link set can0 up type can bitrate 500000
sudo ifconfig can0 up
```

watch the CAN message:

```
candump can0
```