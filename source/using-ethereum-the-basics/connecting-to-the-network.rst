********************************************************************************
Connecting to the Network
********************************************************************************

How to Connect
================================================================================
Geth continuously attempts to connect to other nodes on the network
until it has peers. If you have UPnP enabled on your router or run
ethereum on an Internet-facing server, it will also accept connections
from other nodes.

Geth finds peers through something called the discovery protocol. In
the discovery protocol, nodes are gossipping with each other to find
out about other nodes on the network. In order to get going initially,
geth uses a set of bootstrap nodes whose endpoints are recorded in the
source code.



Checking Connectivity and ENODE IDs
--------------------------------------------------------------------------------
To check how many peers the client is connected to in the interactive console, the ``net`` module has two attributes give you info about the number of peers and whether you are a listening node.

.. code-block:: Javascript

  > net.listening
  true

  > net.peerCount
  4


To get more information about the connected peers, such as IP address and port number, supported protocols, use the ``peers()`` function of the ``admin`` object. ``admin.peers()`` returns the list of currently connected peers.

.. code-block:: Javascript

  > admin.peers
  [{
  	ID: 'a4de274d3a159e10c2c9a68c326511236381b84c9ec52e72ad732eb0b2b1a2277938f78593cdbe734e6002bf23114d434a085d260514ab336d4acdc312db671b',
  	Name: 'Geth/v0.9.14/linux/go1.4.2',
  	Caps: 'eth/60',
  	RemoteAddress: '5.9.150.40:30301',
  	LocalAddress: '192.168.0.28:39219'
   }, {
  	ID: 'a979fb575495b8d6db44f750317d0f4622bf4c2aa3365d6af7c284339968eef29b69ad0dce72a4d8db5ebb4968de0e3bec910127f134779fbcb0cb6d3331163c',
  	Name: 'Geth/v0.9.15/linux/go1.4.2',
  	Caps: 'eth/60',
  	RemoteAddress: '52.16.188.185:30303',
  	LocalAddress: '192.168.0.28:50995'
   }, {
  	ID: 'f6ba1f1d9241d48138136ccf5baa6c2c8b008435a1c2bd009ca52fb8edbbc991eba36376beaee9d45f16d5dcbf2ed0bc23006c505d57ffcf70921bd94aa7a172',
  	Name: 'pyethapp_dd52/v0.9.13/linux2/py2.7.9',
  	Caps: 'eth/60, p2p/3',
  	RemoteAddress: '144.76.62.101:30303',
  	LocalAddress: '192.168.0.28:40454'
   }, {
    ID: 'f4642fa65af50cfdea8fa7414a5def7bb7991478b768e296f5e4a54e8b995de102e0ceae2e826f293c481b5325f89be6d207b003382e18a8ecba66fbaf6416c0',
    Name: '++eth/Zeppelin/Rascal/v0.9.14/Release/Darwin/clang/int',
    Caps: 'eth/60, shh/2',
    RemoteAddress: '129.16.191.64:30303',
    LocalAddress: '192.168.0.28:39705'
   } ]


To check the ports used by geth and also find your enode URI run:

.. code-block:: Javascript

  > admin.nodeInfo
  {
    Name: 'Geth/v0.9.14/darwin/go1.4.2',
    NodeUrl: 'enode://3414c01c19aa75a34f2dbd2f8d0898dc79d6b219ad77f8155abf1a287ce2ba60f14998a3a98c0cf14915eabfdacf914a92b27a01769de18fa2d049dbf4c17694@[::]:30303',
    NodeID: '3414c01c19aa75a34f2dbd2f8d0898dc79d6b219ad77f8155abf1a287ce2ba60f14998a3a98c0cf14915eabfdacf914a92b27a01769de18fa2d049dbf4c17694',
    IP: '::',
    DiscPort: 30303,
    TCPPort: 30303,
    Td: '2044952618444',
    ListenAddr: '[::]:30303'
  }

Common Problems With Connectivity
--------------------------------------------------------------------------------
Sometimes you just can't get connected. The most common reasons are
as follows:

- Your local time might be incorrect. An accurate clock is required
  to participate in the Ethereum network.Check your OS for how to resync
  your clock (example sudo ntpdate -s time.nist.gov) because even 12
  seconds too fast can lead to 0 peers.
- Some firewall configurations can prevent UDP traffic from flowing.
  You can use the static nodes feature or ``admin.addPeer()`` on the console
  to configure connections by hand.

To start geth without the discovery protocol, you can use the `--nodiscover` parameter. You only want this is you are running a test node or an experimental test network with fixed nodes.

Syncing vs Fast Syncing
--------------------------------------------------------------------------------

.. todo::
   Explain syncing vs. fast syncing.


Light Client Network Connectivity
================================================================================

.. todo::
   Explain light client.

Static Nodes, Trusted Nodes, and Boot Nodes
================================================================================

Geth supports a feature called static nodes if you have certain
peers you always want to connect to. Static nodes are re-connected
on disconnects. You can configure permanent static nodes by putting something like
the following into ``<datadir>/static-nodes.json`` (this should be the same folder that your ``chaindata`` and ``keystore`` folders are in)

.. code-block:: Javascript

  [
  	"enode://f4642fa65af50cfdea8fa7414a5def7bb7991478b768e296f5e4a54e8b995de102e0ceae2e826f293c481b5325f89be6d207b003382e18a8ecba66fbaf6416c0@33.4.2.1:30303",
  	"enode://pubkey@ip:port"
  ]


You can also add static nodes at runtime via the Javascript console using `admin.addPeer()`

.. code-block:: Console

  > admin.addPeer("enode://f4642fa65af50cfdea8fa7414a5def7bb7991478b768e296f5e4a54e8b995de102e0ceae2e826f293c481b5325f89be6d207b003382e18a8ecba66fbaf6416c0@33.4.2.1:30303")