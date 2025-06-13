# BGP MED (Multi-Exit Discriminator)
## BGP attribute used to tell the "preferred path" to peers of different ASs to consider which path to take if there are multiple paths to the same AS.

## 🔍 What is BGP MED?

Used to compare routes between 2 routes entering the same AS

Lower MED → Better → Higher priority

Used to tell neighbors (external BGP peers) that “It’s better to enter this way”

📝 Unlike weight or local-preference, which is only within our router or AS


## ✅ When to use it?

For example:

You have 2 links connected to the same ISP (same AS):

Link1: High Speed

Link2: Backup

You want the ISP to choose Link1 as the primary traffic source to send to you → You use a lower MED on Link1 to “tell” the ISP to go through Link1


## 🧪 Example of MED setup
Suppose you have 2 peers from the same AS (ISP1):

Peer1: Primary Link (MED = 10)

Peer2: Secondary Link (MED = 200)

````
/routing/filter/rule
add chain=bgp-out-isp1-pri set-bgp-med=10
add chain=bgp-out-isp1-backup set-bgp-med=200
````

````
/routing/bgp/connection
set [find where .name="peer-isp1-primary"] output.filter=bgp-out-isp1-pri
set [find where .name="peer-isp1-backup"] output.filter=bgp-out-isp1-backup
````



