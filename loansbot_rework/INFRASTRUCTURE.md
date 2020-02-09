# Infrastructure

This repository will contain descriptions of how each EC2 instance was setup
and configured. It's probably not worth it to go with actual initialization
scripts since they will likely never be used. This will also describe the VPC
and route tables.

We'll use a standard public/private subnet approach, where the reverse proxy
is the only thing in the public subnet and everything else is in the private
subnet. The intention is that we never have to SSH directly to the servers
outside of original setup, so rather than adding a NAT gateway for SSH, we'll
just open up that port temporarily during setup then close it down. This is
also cheaper.
