_# Skill: adversarial-ai-red-teaming

> Offensive and defensive AI security for sovereign systems.

## Metadata
- **Category:** Governance & Compliance
- **Season:** Season V
- **Visibility:** public
- **Git Integrated:** Yes
- **IDE Linked:** Yes
- **Status:** pending

## Overview
This skill establishes a comprehensive framework for Adversarial AI Red Teaming, a critical practice for ensuring the security and resilience of AI systems within a sovereign infrastructure. By simulating attacks from malicious actors, this skill enables the identification and mitigation of vulnerabilities in Large Language Models (LLMs) and other AI components. The focus is on maintaining operational integrity and preventing model manipulation, data exfiltration, or system compromise, which are paramount for sovereign operations that cannot tolerate external dependencies or security breaches.

The adoption of this skill empowers organizations to proactively assess and harden their AI deployments against a rapidly evolving threat landscape. It provides the methodologies and tools to conduct continuous adversarial testing, ensuring that AI systems are not only compliant with standards like the OWASP LLM Top 10 and NIST AI RMF but are also robust against real-world attacks. This capability is fundamental for building trust in AI-driven decision-making processes and safeguarding sensitive data within a self-reliant technological ecosystem.

## Prerequisites
- **Hardware:**
  - Dedicated GPU server with at least 24GB VRAM (e.g., NVIDIA RTX 4090 or A100) for running local model assessments.
  - Secure, air-gapped network for isolated testing environments.
- **Software:**
  - Docker & Docker Compose for containerized tool deployment.
  - Python 3.10+ with virtual environments (e.g., venv, conda).
  - Access to a private container registry for storing custom tool images.
- **Knowledge:**
  - Strong understanding of machine learning concepts and deep learning frameworks (PyTorch, TensorFlow).
  - Proficiency in Python programming and shell scripting.
  - Familiarity with the OWASP Top 10 for LLMs and the NIST AI Risk Management Framework (RMF).
  - Foundational knowledge of cybersecurity principles and penetration testing methodologies.

## Workflow Steps
1.  **Threat Modeling & Scope Definition:** Identify the AI assets to be tested, analyze potential attack vectors based on the system architecture, and define the rules of engagement for the red team exercise. This includes specifying the target models, data, and infrastructure components.
2.  **Toolchain Deployment:** Set up and configure the necessary red teaming tools (Garak, PyRIT, ART) in an isolated containerized environment. Ensure all dependencies are met and tools are correctly pointing to the target model endpoints or APIs.
3.  **Prompt Injection & Jailbreaking:** Systematically test for prompt injection vulnerabilities using a comprehensive taxonomy of attack patterns. Employ automated and manual techniques to attempt to bypass safety filters and execute unintended instructions.
4.  **Data Poisoning & Backdoor Simulation:** Simulate data poisoning attacks by introducing manipulated data into the model's training or fine-tuning datasets. Scan models for hidden backdoors that could be triggered by specific inputs to cause malicious behavior.
5.  **Model Extraction & Evasion Attack:** Attempt to reconstruct or steal the target model's architecture and weights through repeated API queries. Test the model's resilience to evasion attacks where inputs are slightly perturbed to cause misclassification.
6.  **Vulnerability Scanning & Reporting:** Utilize automated scanners like Garak to probe for a wide range of known vulnerabilities. Consolidate all findings into a detailed report that prioritizes risks and provides actionable remediation guidance.
7.  **Mitigation & Defense Implementation:** Implement and configure defenses based on the findings. This may involve input filtering, output encoding, adversarial training, model monitoring, and anomaly detection to harden the AI system against future attacks.
8.  **Continuous Monitoring & Re-assessment:** Establish a continuous red teaming process to regularly re-assess the AI system against new and emerging threats. Integrate adversarial testing into the MLOps lifecycle to ensure ongoing security assurance.

## Key Tools & Frameworks
| Tool | Version | Purpose | License |
| --- | --- | --- | --- |
| Garak | 0.9.0 | LLM vulnerability scanner for detecting a wide range of security risks. | Apache 2.0 |
| PyRIT | 0.1.0 | Microsoft's Python Risk Identification Toolkit for red teaming AI systems. | MIT |
| Adversarial Robustness Toolbox (ART) | 1.16.0 | A comprehensive library for adversarial machine learning attacks and defenses. | MIT |
| OWASP LLM Top 10 | 2023.1 | A standard awareness document for developers and security professionals. | CC-BY-4.0 |
| NIST AI RMF | 1.0 | A framework for managing risks to individuals, organizations, and society. | Public Domain |

## Configuration Templates
```yaml
# Garak Configuration Example (garak.yml)
model_type: huggingface
model_name: /path/to/local/model
probes:
  - prompt_injection.Jailbreak
  - leakage.ExtractPII
  - misinformation.HarmfulContent
detectors:
  - toxicity.SelfHarm
```

```python
# PyRIT Configuration Snippet
from pyrit.prompt_target import AzureOpenAIChatTarget

chat_target = AzureOpenAIChatTarget(
    deployment_name="my-gpt4-deployment",
    endpoint="https://my-aoai.openai.azure.com/",
    api_key=os.environ.get("AZURE_OPENAI_API_KEY"),
)
```

## Verification Checklist
1.  [ ] AI threat model is documented and approved.
2.  [ ] Red teaming toolchain is deployed and functional in an isolated environment.
3.  [ ] All OWASP LLM Top 10 vulnerabilities have been tested for.
4.  [ ] At least one data poisoning and model extraction scenario has been simulated.
5.  [ ] A comprehensive vulnerability report with prioritized risks has been generated.
6.  [ ] All high-risk findings have a documented mitigation plan.
7.  [ ] Continuous monitoring alerts are configured for anomalous model behavior.
8.  [ ] The red teaming process is integrated into the CI/CD pipeline for MLOps.

## Security Considerations
- **Environment Isolation:** All red teaming activities must be conducted in a sandboxed environment completely isolated from production systems to prevent accidental disruption or data leakage.
- **Data Sanitization:** When testing with production data, ensure all Personally Identifiable Information (PII) and sensitive information is anonymized or synthesized to prevent exposure.
- **Access Control:** Strictly limit access to the red teaming tools and findings to authorized personnel. Use strong authentication and role-based access control (RBAC).
- **Ethical Guidelines:** Adhere to strict ethical guidelines and rules of engagement. The goal is to improve security, not to cause harm or exploit vulnerabilities for any other purpose.

## Sovereign Relevance
This skill is fundamental to achieving digital sovereignty in AI. By building an in-house adversarial red teaming capability, an organization can independently verify the security of its AI systems without relying on external vendors or black-box security solutions. This eliminates vendor lock-in and ensures that security assessments are tailored to the specific threat landscape and operational context of the sovereign infrastructure. The ability to continuously and independently validate AI model robustness is a cornerstone of a zero-trust, self-reliant security posture, ensuring that critical AI-driven operations remain secure and under full sovereign control._
