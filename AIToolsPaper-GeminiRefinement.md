An analysis of the provided document, fine-tuned for a technology industry audience and presented in a blog post format.

---

### A Case Study in the Practical Limits of AI Developer Tools

We are solidly in the age of AI helpers being part of solving technology problems. The pressing question is no longer _if_ we should use them, but for which problems they are truly effective, and which models excel at different tasks. The landscape of technology challenges is vast, ranging from routine coding issues to designing large, interconnected systems that handle massive amounts of diverse data.

The initial wave of AI tools capably handled writing moderate amounts of code for well-defined requirements. Tasks like predicting the next line of code, implementing a described algorithm, or reformatting code to meet style guides are now fundamental capabilities for these AI assistants. For these simpler tasks, the choice of programming language is often not a significant factor, as long as a substantial body of examples exists for the AI to learn from.

This post presents a case study that explores the effectiveness of various AI helper tools when applied to a consistent, real-world cloud engineering problem across the three major cloud providers: AWS, Google Cloud, and Azure. The goal was to set up a simple static website and configure a CI/CD pipeline using modern, secret-free OIDC authentication—a common, yet non-trivial task.

---

### The Case Study: A Journey Through the Clouds

The real test for these AI tools begins with more specialized use cases. As a cloud engineer frequently working with AWS, Azure, and Google Cloud, Terraform is my common language for provisioning resources. While the basics of Terraform's HCL are simple, the complexity emerges when you ask an AI assistant to create a template for a specific set of interconnected resources. A key area of difficulty is Identity and Access Management (IAM), where each cloud provider uses similar concepts but implements them differently. To be effective, the AI must possess a deep knowledge of each platform's documentation to generate a useful template for specific needs.

#### **Amazon Web Services (AWS): The Path of Least Resistance**

For AWS, the experience was relatively smooth. When tasked with creating an OIDC connection for a deployment pipeline with CircleCI, the AI prompts provided a decent starting point. After some adjustments, I discovered a publicly available Terraform module that accomplished the task with just eight lines of code. The AWS solutions were generally stable and sensible, which can be attributed to both AWS's simpler approach for certain use-cases and the vast community of engineers creating and sharing integrations.

When asked to create a full configuration for an AWS CloudFront site—including certificates, S3 storage, and logs—Google's Gemini provided an almost perfect solution on the first try. The necessary refinements were minor tweaks, not fundamental fixes. This success is likely due to CloudFront encapsulating most of the configuration into a single resource, providing a wealth of straightforward examples for the AI to draw upon.

#### **Google Cloud Platform (GCP): A Mixed Bag**

The experience with Google Cloud was more varied. While setting up the OIDC connection with CircleCI was similar to AWS—the raw Terraform was a bit messy, but ultimately a reusable module simplified it to eight lines—other tasks proved more challenging.

Attempting to create a simple static website using a CDN approach on GCP resulted in resources that were extremely messy to connect. Google's own Gemini struggled to make sense of the issue and failed to generate useful code. At one point, ChatGPT even hallucinated a non-existent resource (`google_certificate_manager_target_https_proxy`) in an attempt to solve a certificate problem. While there was some decent support from the Terraform community in certain areas, the AI tools came up short on this particular challenge.

#### **Azure: The Outlier**

Getting Azure to work with a CircleCI deployment pipeline using OIDC proved to be a major challenge. All the AI models I tried produced "valid" but ultimately unusable Terraform resources for this integration. This difficulty was partly due to the smaller user base where Azure and CircleCI intersect, meaning no easy-to-use Terraform module was available. The AI suggestions consistently defaulted to generating static credentials, an approach I was determined to avoid.

In the end, I devised a solution, but the AI tools were only helpful for small, incremental changes, like translating an Azure command-line snippet into a Terraform resource. The core problems were rooted in how Azure manages resource "ownership" during deployments and its rigid OIDC subject mapping. To resolve the ownership issue and prevent conflicts between local plans and CI/CD deployments, I had to hardcode my owner ID into the template. For the OIDC subject matching, I had to deploy a separate Federated Credential for each individual CircleCI project because Microsoft's more flexible "Match Expression" feature is not yet supported for CircleCI in the Terraform provider.

Interestingly, while working within the Azure console, the integrated CoPilot did perform reasonably well at troubleshooting. This suggests that when a platform's resource model is particularly complex or esoteric, the knowledge base for AI tools to draw from may not have reached a critical mass, making the platform itself difficult to figure out even for AI assistants.

~~Interestingly, while working within the Azure console, the integrated CoPilot did perform reasonably well at troubleshooting. This suggests that when a platform's resource model is particularly complex or esoteric, the knowledge base for AI tools to draw from may not have reached a critical mass, making the platform itself difficult to figure out even for AI assistants.~~

**refined, prompt: In the paragraph about the azure console, the 2 concepts connected here are not related. Re-write this paragraph to show that while the console CoPilot was useful in the console, overall the AI tools did poorly generating templates for azure resources**

While the integrated CoPilot was reasonably helpful for troubleshooting directly within the Azure console, this success was an isolated bright spot. The broader task of generating functional Terraform templates with other AI tools proved to be a major challenge. This discrepancy suggests a larger issue: the AI models struggled to generate useful infrastructure-as-code for Azure because the platform's resource model is either too esoteric to have a critical mass of public content for the AI to learn from, or the platform itself is simply too difficult for the current generation of AI to understand and model effectively.

---

### Summary: The State of AI Helpers

This cross-platform case study highlights the varying capabilities of current AI helper tools when faced with different levels of technological challenges.

- **Small Challenges:** For small, well-defined tasks, the AI tools performed admirably. This includes scaffolding code, reformatting it, and implementing simple, self-contained algorithms. Creating a full AWS CloudFront site configuration was also successful, likely because it could be treated as a single, well-documented unit. All the tools used were effective at creating and refining these narrow parts of the desired solutions.

- **Medium Challenges:** Medium-sized challenges, such as setting up OIDC connections, revealed the importance of community support. For AWS and GCP, the existence of pre-built Terraform modules made the AI's initial, sometimes messy, code less of an issue. The AI could get you started, but the robust community provided the optimal solution.

- **Large Challenges:** The case study demonstrated that none of the AI tools were particularly adept at creating big-picture, cross-platform solutions from scratch. The Azure integration was a prime example of a large challenge where the AI helpers struggled significantly. The combination of a less common technology pairing (Azure and CircleCI), platform-specific complexities like ownership and OIDC subject mapping, and the lack of community-provided modules pushed the problem beyond the capabilities of the AI tools. The final solution required significant manual intervention and deep domain knowledge, with the AI only providing minor, incremental assistance.

In conclusion, while AI helper tools are powerful allies for developers, their effectiveness is currently bounded by the complexity of the task and the breadth of available knowledge for them to learn from. They excel at contained, well-documented problems but falter when faced with the intricate, multi-faceted challenges that often arise in real-world, cross-platform engineering.
