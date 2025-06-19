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

In the blink of an eye, Generative AI has moved from the realm of science fiction to an indispensable tool in our daily lives. 

From crafting compelling marketing copy to generating intricate code, or even aiding in creative brainstorming, Large Language Models (LLMs) like GPT, Llama, and Gemini have opened up a universe of possibilities. They are reshaping how we work, create, and interact with information, empowering us with capabilities that were unimaginable just a few years ago

For many, interacting with these powerful AI models means tapping into cloud-hosted APIs – a convenient, scalable, and readily accessible solution. But what if you crave more? More control, more customization, more privacy, or simply a deeper understanding of the magic behind the curtain?

This is where the exciting world of self-hosting your own Large Language Models on Google Kubernetes Engine (GKE) comes into play. Moving beyond simple API calls, self-hosting on GKE means bringing these formidable computational brains into your own managed infrastructure, offering unparalleled flexibility and governance.

Deploying Large Language Models (LLMs) in production requires a robust and scalable infrastructure. These models are inherently resource-intensive, demanding significant computational power, efficient data handling, and sophisticated management. 

## Google Kebernetes Engine (GKE)

While utilizing a managed LLM endpoint like Google's Gemini offers undeniable convenience, deploying an open model such as Gemma 2 directly on your GKE cluster provides distinct advantages, especially for advanced use cases:

* **Unparalleled Control:** Gain complete autonomy over the model's lifecycle, resource allocation, and scaling strategies. This level of granular control is essential for applications with stringent performance, security, or compliance mandates.

* **High Computational Power:** GKE offers first-class support for attaching NVIDIA GPUs and Google's custom TPUs to your clusters. You can easily provision node pools with specific GPU types. It also alows you to optimize GPU utilization by sharing GPUs among multiple containers or leveraging NVIDIA's Multi-instance GPU (MIG) technology to partition a single GPU into multiple smaller, isolated instances.

* **Significant Cost Optimization:** For scenarios involving high-volume inference or continuous usage, operating your own LLM instance on GKE can prove substantially more cost-efficient than relying on per-token API charges. GKE allows you to use Spot VMs (Google Cloud's version of preemptible VMs) for your node pools, offering significant cost savings (up to 91%) for fault-tolerant inference workloads.

* **Enhanced Data Locality and Privacy:** Maintain your sensitive data and the model itself within your controlled private cloud environment. This is a critical factor for organizations navigating strict regulatory compliance requirements and prioritizing data sovereignty.

*  **Observability and Monitoring:** GKE seamlessly integrates with Google Cloud's native monitoring and logging services, providing out-of-the-box dashboards, metrics, and log aggregation for your cluster, nodes, and pods. You can expose custom metrics from your LLM serving applications to Cloud Monitoring, allowing for fine-grained autoscaling and performance analysis

* **Accelerated Experimentation and Innovation:** Freely explore and integrate the latest advancements in LLM research and deployment techniques. Self-hosting removes API limitations, fostering rapid experimentation with cutting-edge features and models.

## GKE Inference Gateway

Effective load balancing stands as the cornerstone of any reliable and high-performing application. Serving as the vital traffic controller, it intelligently and uniformly distributes incoming requests across all available resources, thereby eliminating bottlenecks and ensuring an uninterrupted, smooth user experience.

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

In the evolving landscape of Generative AI, security is not just an add-on; it's a fundamental necessity. As organizations increasingly deploy powerful LLMs for critical applications, ensuring the safety, integrity, and responsible use of these models becomes paramount. This is where Google Cloud's Model Armor steps in. Model Armor is a specialized service designed to provide a crucial layer of AI safety and security for your generative models. It acts as a protective shield, integrating directly into your inference pipeline to scrutinize prompts and responses, safeguard against malicious inputs or harmful outputs, and provide the visibility needed for continuous improvement in AI safety

![Model Armour](https://cloud.google.com/static/security-command-center/images/model-armor-architecture.svg)

* **Robust AI Safety and Security:** Model Armor empowers organizations to proactively mitigate the security and safety risks inherent in LLM deployments. It specifically addresses critical concerns like prompt injection, "jailbreak" attempts, the creation of harmful content, exposure to malicious URLs, and accidental sensitive data leakage, ensuring secure and dependable LLM integration into products and services.
* **Centralized Management and Oversight:** Offering unified control across all LLM applications, Model Armor enables CISOs and security architects to effectively monitor and manage comprehensive security and safety policies from a single point.
* **Versatile Deployment:** Model Armor is designed for flexibility, supporting multi-cloud, multi-model, and multi-LLM environments. Its adaptable deployment options allow seamless integration at various points within the LLM application architecture, fitting into diverse existing infrastructures and workflows.
* **Customizable Policies and Seamless Integration:** Tailor Model Armor's policies to align precisely with unique application requirements and integrate it effortlessly into current operational processes.
