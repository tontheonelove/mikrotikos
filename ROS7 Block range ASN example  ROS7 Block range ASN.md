## ROS7 Block range ASN

### example

``````
/routing filter num-list
add comment="Deny-ASN" list=Deny-ASN range=131293-131300
/routing filter rule
add chain=your_chain disabled=no rule="if (bgp-as-path [[:Deny-ASN:]]) {reject}"
``````
