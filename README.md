## AKS - Kubernetes Wordpress Server
### Project Overview:
A Kubernetes cluster running a content management site, with a web front-end (Wordpress) running on one of the containers and a SQL database/back-end running on another. To deploy, please reference deployment.md

### Data Flow Diagram: (wip)
### Architecture Diagram: (wip)
### Services Being Used:
Azure Kubernetes Service(AKS) - hypervisor and container instances
### Adherence to Azure's Well Architected Framework:
- Reliability:
  - Auto-scaling and load-balancing if usage exceeds a certain threshold
- Security:
  -SSH and other non-used ports closed for outside traffic, secrets generation for SQL server
- Cost Optimization:
  - Standard B1s VMs, Basic 'free' tier Kubernetes, auto-scaling, and limited node pools to minimize cost
- Operational Excellence:
  - Auto-scaling, load-balancing, and deployment can be automated through provided methods
- Performance Efficiency:
  - Auto-scaling, load-balancing, and appropriate VM sku for the provided outcomes
### Future Revisions:
- Add additional content to the webpage itself, because it is currently very bare bones. Not that I know what to put on it, but that's beside the point.
- Deploy a website of my own onto the service rather than go through Wordpress.

*(Written for ST:Cloud Computing - Indiana Tech IS4990)*
