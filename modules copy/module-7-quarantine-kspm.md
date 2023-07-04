# Module 7 - Quarantine a Workload and KSPM

## Quarantine a Workload

Suppose you have a compromised workload in your environment and want to conduct further investigation on it. In that case, you should not terminate the workload but isolate it, so it will not be able to cause damage or spread laterally across your environment. In this situation, you should quarantine the pod by applying a security policy to it that will deny all the egress and ingress traffic and log all the communications attempts from and to that pod.

We have the `quarantine` policy created in the `security` tier. This policy has a label selector of `quarantine = true`. Let's see how it works.

1. Execute the following commands from the attacker pod (if you did quit from its shell, it got deleted. Create it again if it's the case.).

   - Test the connection to a local service

     ```bash
     curl -m3 http://vote.vote
     ```

   - Test the connectivity with the Kubernetes API

     ```bash
     curl -m3 -k https://kubernetes:443/versions
     ```  

   - Test the connectivity with the internet

     ```bash
     curl -m3 http://neverssl.com
     ```  

2. Label the attacker pod with `quarantine = true`. 

   ```bash
   kubectl label pod attacker quarantine=true
   ```

3. Repeat the tests from step 1. Now, as you can see, the cannot establish communication with any of the destinations.

---

## Visualize security posture of your Kubernetes cluster 


### Timeline

What changed, who did it, and when? This information is critical for security. Native Kubernetes doesn’t provide an easy way to capture audit logs for pods, namespaces, service accounts, network policies, and endpoints. The Calico Cloud timeline provides audit logs for all changes to network policy and other resources associated with your Calico Cloud deployment.

1. On the Calico Cloud GUI, navigate to `Activity` and explore the entries in the `Timeline`.

![timeline](https://github.com/tigera-solutions/cc-aks-detect-block-network-attacks/assets/104035488/27bfeaff-4c1a-4d3d-b5c4-5234ecb13a52)

### Compliance Reports

Continuous compliance means employing a continual audit that shows what traffic was allowed in your infrastructure, what traffic was denied and why, and logs of who was trying to change what and whether those changes went into effect. Continuous compliance allows teams to pinpoint any point in time, say with reasonable certainty, whether the organization was compliant, and provide documentation to prove it. Calico’s compliance reports visually describe the security controls in place in an easy-to-understand policy view. Calico also shows all workloads that are in-scope and out-of-scope with your policy.

1. On the Calico Cloud GUI, navigate to `Compliance`.

![compliance-reports](https://user-images.githubusercontent.com/104035488/192358634-c873ffb5-f874-495f-8ba4-79806ff84654.gif)


2. Explore the Compliance Reports.

![cis-benchmark](https://user-images.githubusercontent.com/104035488/192358645-ab77c305-0a9d-4242-b37f-972dc22b4d84.gif)


**Congratulations! You completed this workshop!**

--- 

[:arrow_right: Module 8 - Clean up](/modules/module-8-clean-up.md)  <br>

[:arrow_left: Module 6 - Realtime Container Thread Defence](/modules/module-6-threat-defence.md)  
[:leftwards_arrow_with_hook: Back to Main](/README.md)  
