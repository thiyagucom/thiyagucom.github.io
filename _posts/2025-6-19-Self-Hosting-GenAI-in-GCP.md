---
title: "Self Hosting Generative AI Models in Google Cloud"
categories:
  - Architect
classes: wide
tags:
  - Google Cloud
  - Hybrid Cloud
  - Security
  - Generative AI
  - GenAI
  - GKE
  - Google Kubernetes Engine
  - Kubernetes
  - LLM
  - Inference Gateway
  - Model Armour 
---

Generative AI, once just a sci-fi dream, is now a crucial part of our everyday lives. Tools like GPT, Llama, and Gemini can do amazing things, from writing marketing materials and computer code to helping us brainstorm ideas. They're changing how we work, create, and find information, giving us abilities we couldn't have imagined just a few years ago.

While utilizing a managed LLM endpoint like Google's Gemini offers undeniable convenience, what if you want more? What if you want more control, the ability to customize things, better privacy, or just to understand how it all works?

That's where running your own Large Language Models on Google Kubernetes Engine (GKE) becomes exciting. Instead of just using online tools, self-hosting on GKE means you bring these powerful AI brains into your own managed system. This gives you unmatched flexibility and control over how they operate.

## Google Kubernetes Engine (GKE)

Deploying an open model such as Gemma 2 directly on your GKE cluster provides distinct advantages, especially for advanced use cases:

* **You're in Charge:** You get total control over how your AI model runs, how much computing power it uses, and how it grows. This is very important for apps that need high performance, security, or have strict rules to follow.

* **Lots of Power:** GKE is great at using powerful computer chips like NVIDIA GPUs and Google's own TPUs. You can easily set up your systems with the right kind of chips. Plus, you can make the most of these chips by sharing them among different tasks or even splitting one big chip into smaller, separate ones.

* **Save Money:** If you're using AI a lot or all the time, running your own LLM on GKE can be much cheaper than paying for each bit of data you send to an online AI service. GKE also lets you use "Spot VMs," which are much cheaper (you can save up to 91%!) for AI tasks that don't need to run perfectly all the time.

* **Keep Your Data Close and Private:** You can keep all your sensitive information and the AI model itself within your own cloud and specific region. This is a big deal for companies that have strict rules about data privacy and where their data is stored.

* **Easy to Monitor:** GKE works smoothly with Google Cloud Observability tools for monitoring and logging. This means you get pre-made and custom dashboards, performance metrics, and logs for your entire system. You can even track specific things about your LLM to help it automatically adjust and analyze how well it's doing.

* **Faster Upgrade and New Ideas:** You're free to try out the newest developments in AI without any limits from online services. Running your own models means you can quickly experiment with the latest features and AI models.

## GKE Inference Gateway

Effective load balancing stands as the cornerstone of any reliable and high-performing application. Serving as the vital traffic controller, GKE Inference Gateway intelligently and uniformly distributes incoming requests across all available resources, thereby eliminating bottlenecks and ensuring an uninterrupted, smooth user experience.

GKE Inference Gateway, an extension to the GKE Gateway, offers optimized routing and load balancing specifically designed for serving Generative AI (GenAI) workloads. It significantly simplifies the deployment, management, and observability of AI inference workloads.

![GKE Inference Gateway](https://cloud.google.com/static/kubernetes-engine/images/request-flow.png)

GKE Inference Gateway offers several key capabilities to enhance Generative AI workload serving:

* **Optimized Inference Load Balancing:** It intelligently distributes requests to maximize AI model serving performance. By leveraging metrics such as KVCache Utilization and the queue length of pending requests from model servers, it ensures more efficient use of accelerators like GPUs and TPUs for generative AI workloads.
* **Dynamic LoRA Fine-tuned Model Serving:** The gateway supports serving dynamic LoRA fine-tuned models on a shared accelerator. This innovative feature significantly reduces the number of GPUs and TPUs required by multiplexing multiple LoRA models on a single base model and accelerator.
* **Optimized Inference Autoscaling:** It empowers the GKE Horizontal Pod Autoscaler (HPA) to use model server metrics for autoscaling decisions. This guarantees efficient compute resource utilization and consistently optimized inference performance.
* **Model-Aware Routing:** Inference requests are routed based on model names defined within the OpenAI API specifications within your GKE cluster. This allows for flexible Gateway routing policies, including traffic splitting and request mirroring, to effectively manage different model versions and simplify model rollouts. For instance, requests for a specific model name can be directed to various InferencePool objects, each serving a distinct model version.
* **Model-Specific Serving Criticality:** This feature enables you to assign a serving criticality level to your AI models. You can prioritize latency-sensitive requests over latency-tolerant batch inference jobs, ensuring critical applications receive preferential resource allocation, even when resources are constrained.
* **Integrated AI Safety:** GKE Inference Gateway integrates with Google Cloud Model Armor, a service that applies essential AI safety checks to both prompts and responses at the gateway level. Model Armor also provides comprehensive logs of requests, responses, and processing details for retrospective analysis and optimization. Furthermore, the gateway's open interfaces allow for seamless integration of custom services from third-party providers and developers into the inference request pipeline.
* **Inference Observability:** It provides rich observability metrics for inference requests, including request rate, latency, errors, and saturation. These metrics are crucial for monitoring the performance and behavior of your inference services.

## Model Armour

As organizations increasingly deploy powerful LLMs for critical applications, ensuring the safety, integrity, and responsible use of these models becomes paramount. This is where Google Cloud's Model Armor steps in. 

Model Armor is a special service that makes your AI models safer and more secure. Think of it like a protective shield for your AI. It checks what you ask the AI (prompts) and what the AI says back (responses) to make sure there's nothing harmful or malicious. This also helps you see how to keep making your AI safer over time.

![Model Armour](https://cloud.google.com/static/security-command-center/images/model-armor-architecture.svg)

* **Robust AI Safety and Security:** Model Armor empowers organizations to proactively mitigate the security and safety risks inherent in LLM deployments. It specifically addresses critical concerns like
  * Prompt Injection
  * "jailbreak" attempts
  * The creation of harmful content
  * Exposure to malicious URLs 
  * Accidental sensitive data leakage

  to ensure secure and dependable LLM integration into products and services.
* **Centralized Management and Oversight:** Offering unified control across all LLM applications, Model Armor enables CISOs and security architects to effectively monitor and manage comprehensive security and safety policies from a single point.
* **Versatile Deployment:** Model Armor is designed for flexibility, supporting multi-cloud, multi-model, and multi-LLM environments. Its adaptable deployment options allow seamless integration at various points within the LLM application architecture, fitting into diverse existing infrastructures and workflows.
* **Customizable Policies and Seamless Integration:** Tailor Model Armor's policies to align precisely with unique application requirements and integrate it effortlessly into current operational processes.

## Conclusion

Deploying Large Language Models (LLMs) in production demands a robust and scalable infrastructure. These models are inherently resource-intensive, requiring significant computational power, efficient data handling, and sophisticated management. 

**Google Cloud** helps enterprises achieve these critical requirements with powerful services such as Google Kubernetes Engine (GKE), the specialized Inference Gateway for optimized serving, and Model Armor for comprehensive AI safety and security.

## Resources:

* https://cloud.google.com/blog/products/application-development/choosing-a-self-hosted-or-managed-solution-for-ai-app-development
* https://cloud.google.com/kubernetes-engine/docs/concepts/about-gke-inference-gateway
* https://cloud.google.com/security-command-center/docs/model-armor-overview
