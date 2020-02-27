## Red Teaming

### What is Red Teaming?
* A red team is a group that helps organizations to improve themselves by providing opposition to the point of view of the organization that they are helping.(https://en.wikipedia.org/wiki/Red_team)
* Red teaming is the practice of rigorously challenging plans, policies, systems and assumptions by adopting an adversarial approach.(https://whatis.techtarget.com/definition/red-teaming)
> Red teaming is just not limited to cyber security field and can be relevant to many other fields. Although in this blog post I'll be discussing only in context of Cyber Security field.

### What is Red Teaming in contenxt of Cyber Security?
* Red Teaming is a full scope attack simulation which is planned and designed similar to a real-life adversary to measure how well the company's defensive measures can withsatand. The full scope refers to company's networks, applications, people and physical premise.
* A red team test will challenge a company's network defense systems, employee policies (in terms of cyber security) and physical security of the premise.

### Different Engagement Activities of Red Teaming
* Defining Scope and Rules of Engagement
> This part defines the the target scope, there might be some exceptions to the scope depending on client requirements.
> Getting the authorization from the client to conduct the adversarial attack simulation.
* Reconnaissance
> This phase involves gathering information about target organization. The information is used to map the attack surface and later to determine the attack vectors.
> Examples of the gathered information- Network IP addresses related to the organization, public facing network devices, employees working in the organization, information about physical premise etc.
* Planning and Executing Attack
> In this phase, specific attack vectors are determined based on the reconaissance of the target.
> The determined attack vectors are then used to gain access to the assets of the organization.
> Further attacks like lateral movement, privilege escalation are performed to gain maximum access to the assets of the target organization.
* Documentation and Reporting
> This is the last phase of a red team engagement.
> A final detailed report is created which contains the information about discovery of weaknesses in the system, exploitability of those weaknesses, impact upon the exploitation, mitigation techniques, etc.
> All the things can be very useful to the oraganization to improve their security.

### Skills and Relevant Roles in a Red Team
* Penetration Tester
> Penetration tester is the person who finds and exploits the vulnerabilities in the systems.
> Skills relevant to the role are information gathering, vulnerability assessment, penetration testing.
* Infrastructure and Systems Management
> This skill is one of the very important as an infrastructure engineer is responsible for developing and deployment of command and control infrastructure (which includes c2 servers, redirectors, phishing and payload servers, etc.)
* Development
> A developer is responsible for development of C2 softwares, shellcodes, payloades, exploits etc. 
> For a good red team engagement, using off the shelf tools is not ideal as they can be detected by blue team pretty easily. In order to avoid detection, custom tools must be developed and used.
* Social Engineering
> In order to test employee policies regarding cyber security, various social engineering attacks can be performed. 
* Physical Security
> To test the physical security of the premise of the organization, a red teamer can have skills like lock picking and physical security.
* Technical Writing
> In order to convey the details of the attack and its imoact to the client, a red teamer must have good technical writing skills.
> A good report can also demonstrate the skills of the team more effectively.

### Conclusion
> This blog post was aimed to give a basic idea of what red team is and what are skills and roles associated to a red team. 
