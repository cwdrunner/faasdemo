# faasdemo
This is one of the home lab projects. Initially this README contains a general description and some of the issues encountered so they can be avoided next time. 

## architecture
The home lab consists of the following components:

- 4 Raspbery Pi 4b with 4Gb Ram. Arm CPU. 32 Gb micro SD cards. POE Hats, This group acts as the K3s cluster
- 2-4 port POE Gb unmanaged routers. Obviously an 8 or 16 port router would be better but the lab evolved.
- A spare HP laptop. AMD CPU. The laptop became a major controller as a lot of things wouldn't run on ARM and so it was easier to move forward. 
Future projects could push the technology out. 
- Misc other Pis for NAS, Home Automation, monitoring the aquarium. 


## The process
Basically, one of many processes for installing k3s on Pi and then for installing faascli. 

Issue #1 - Doing faascli build creates the Dockerfile which when run pushes the container to a repo. Something about this setup requires that the Docker User is prepended to the pushed image name. So  cwdrunner/test:lastest. 

Issue #2 - Since the faascli commands are run on the laptop and not one of the pis that are part of the cluster you have to make sure the faas gateway host in the cluster is part of the deployment. Many tutorials us 127.0.0.1 or localhost as the gateway address. Don't do it. 


