# faasdemo
This is one of those home lab projects. Initially this README contains a general description and some of the issues encountered so they can be avoided next time. i'll refine it to be more complete so stop back. 

## architecture
The home lab consists of the following components:

- 4 Raspberry Pi 4b with 4Gb Ram. Arm CPU. 32 Gb micro SD cards. POE Hats, This group acts as the K3s cluster. Static IP addresses. 192.168.10.11-192.168.10.14
- 2-4 port POE Gb unmanaged routers. Obviously an 8 or 16 port router would be better but the lab evolved.
- A spare HP laptop. AMD CPU. The laptop became a major controller as a lot of things wouldn't run on ARM and so it was easier to move forward. 
Future projects could push the technology out. Static IP address to make it easy to find.
- Another Pi configured for NAS. Static IP address. Running OpenMedia Vault. Connected to spare external USB hard drive. 192.168.10.2
- Other misc Pis for Home Automation, monitoring the aquarium.
- A leftover Lynksys wireless router connected to either the cable modem or another router. This keeps the home lab on it's own network. Since this is a wireless router it's possible to login to the network from another laptop or to take the hp mobile to do work.


## The process
Basically, one of many processes for installing k3s on Pi and then for installing faascli. 

Issue #1 - Doing faascli build creates the Dockerfile which when run pushes the container to a repo. Something about this setup requires that the Docker User is prepended to the pushed image name. So cwdrunner/test:lastest. This show up as an authorization error. Google searches tend to dns. no. 

Issue #2 - Since the faascli commands are run on the laptop and not one of the pis that are part of the cluster you have to make sure the faas gateway host in the cluster is part of the deployment. Many tutorials us 127.0.0.1 or localhost as the gateway address. Don't do it. 

## Ansible Tower Project
Now that the first OpenFaas functions have been deployed manually, this project will evolve into deploying, monitoring, and undeploying the functions using Ansible towner installed on the HP laptop. 

