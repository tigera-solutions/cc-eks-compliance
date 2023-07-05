# AWS Dev Days: Hands-on workshop: <br> Achieving PCI, SOC2, HIPAA, NIST and GDPR compliance for containerized applications

## Welcome

In this EKS-focused workshop, you will work with AWS and Calico Cloud to learn how to design and deploy best practices to achieve compliance with regulatory frameworks such as PCI, SOC2 , HIPAA and others and secure your Kubernetes environment. 

Enterprises developing compliant cloud-native applications have two primary needs. First, they must secure and govern access to containerized workloads and the Kubernetes environment. Second, they need to simplify audit logging and compliance reporting. The Kubernetes environment is dynamic and distributed, and workloads are ephemeral, making it difficult to enforce compliance controls and provide continuous reporting.

The purpose of this repository is to offer you a comprehensive, step-by-step guide on creating an AWS EKS cluster, registering it on Calico Cloud, and securing your cloud-native applications to meet compliance requirements. Although Calico Cloud provides a wide range of functionalities and security functionalities, due to time constraints, this workshop will concentrate on a few key security features that effectively ensure compliance across multiple regulatory frameworks. If you would like to delve deeper into this topic, please don't hesitate to contact us. 

## Time Requeriments

The estimated time to complete this workshop is 90-120 minutes.

## Target Audience

- Cloud Professionals
- DevSecOps Professional
- Site Reliability Engineers (SRE)
- Solutions Architects
- Anyone interested in Calico Cloud :)

## Learning Objectives

Learn how to:
- **Scan container images** and **block deployment** based on your security criteria during build time.
- Preview and **enforce security policies** to protect vulnerable workloads.
- Implement **zero-trust access controls** to prevent egress and lateral movement during runtime.
- Implement **runtime security** with IDS/IPS, WAF, and malware detection.
- Get **visibility** into Kubernetes cluster traffic to **troubleshoot** and **improve security**.

## Modules

This workshop is organized in sequencial modules. One module will build up on top of the previous module, so please, follow the order as proposed below.

Module 1 - [Prerequisites](/modules/module-1-prereq.md)  
Module 2 - [Create an EKS cluster](/modules/module-2-create-eks.md)  
Module 3 - [Connect the EKS cluster to Calico Cloud](/modules/module-3-connect-calicocloud.md)  
Module 4 - [Scan Container Images](/modules/module-4-scan-images.md)  
Module 5 - [Security Guardrails for Network-based Threats](/modules/module-5-security-guardrails.md)  
Module 6 - [Configuring IDS protection and Workload-Centric WAF](/modules/module-6-ids-waf.md)  
Module 7 - [Quarantine Infected Workloads and KSPM](/modules/module-7-quarantine-kspm.md)  
Module 8 - [Clean up](/modules/module-8-clean-up.md)  

--- 

### Useful links

- [Project Calico](https://www.tigera.io/project-calico/)
- [Calico Academy - Get Calico Certified!](https://academy.tigera.io/)
- [O’REILLY EBOOK: Kubernetes security and observability](https://www.tigera.io/lp/kubernetes-security-and-observability-ebook)
- [Calico Users - Slack](https://slack.projectcalico.org/)

**Follow us on social media**

- [LinkedIn](https://www.linkedin.com/company/tigera/)
- [Twitter](https://twitter.com/tigeraio)
- [YouTube](https://www.youtube.com/channel/UC8uN3yhpeBeerGNwDiQbcgw/)
- [Slack](https://calicousers.slack.com/)
- [Github](https://github.com/tigera-solutions/)
- [Discuss](https://discuss.projectcalico.tigera.io/)

> **Note**: The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how Calico Cloud can be configured to build a functional solution. These examples are not intended for use in production environments.


