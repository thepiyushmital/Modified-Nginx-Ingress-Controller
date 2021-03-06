# modified-nginx-ingress-controller-daemonset

Nginx ingress controller is one of the most popular ingress controllers for AWS EKS.

The standard deployment of NGINX ingress controller upon AWS can be found in the link below:
https://kubernetes.github.io/ingress-nginx/deploy/

When deploying the ingress controller on any given system, the pod deployment strategy has to be carefully considered. If only one instance of pod is left running in a multi subnet EKS cluster, then on the off chance that the subnet goes down, the would be a considerable system downtime, just because of the unavailablilty of the nginx controller pod.

If **EKS node group** has been setup over a **VPC supporting multiple subnets**, then ideally, **each subnet should have 1 pod** of nginx-ingress-controller running on one of the nodes launched in that subnet. To do so, there are 2 methods. 

1. **No of Ingress-controller pods > No of subnets**
   This approach requires the addition of anti-affinity in the ingress-controller deployment yaml script. Even after adding that, there is no certainty that each pod will be          launched in a different subnet. This strategy can most probably be used just as a stop-gap arrangement, wherein we can hope that the pods are being deployed in atleast 2          subnets.
   
2. **Convert nginx-ingress-controller deployment into a daemonset**
   This strategy would ensure that even in the advent of subnet downtime, the system would be able to process the load with HA. 
   The problem with this strategy lies in the fact that each nginx ingress controller pod requires around 100Mi of memory, which might not be feasible in low memory EC2 instances 
   
This repository provides an updation to the original yaml for deploying ingress-controller resources (including AWS NLB), converting nginx-ingress-controller deployment into nginx-ingress-controller daemonset.

**Link to the original yaml:**
https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.44.0/deploy/static/provider/aws/deploy.yaml
