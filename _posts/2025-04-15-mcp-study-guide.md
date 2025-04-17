---
title: Model Context Protocol study guide
date: 2025-04-15 13:00:00 +0400
categories: [ai, mcp]
tags: [ai, mcp, mcp-server, llm, study-guide]
image:
  path: assets/img/posts/20250415/mcp-thumb.webp
  lqip: assets/img/posts/common/common-thumb-lqip.webp
---

In this blog, I’ve gathered a collection of articles, blogs, and other documentation that provide an in-depth look at the Model Context Protocol basics, including how it functions, its benefits, architecture, and core principles. Additionally, I’ve included a curated list of related interesting readings to further explore these topics.

## Why MCP? What is MCP?

- [Why MCP? (Official MCP Specification)](https://modelcontextprotocol.io/introduction#why-mcp%3F) 
- [Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) by Anthropic   
- [Why MCP Matters](https://opencv.org/blog/model-context-protocol/#h-why-mcp-matters) by sandeep, OpenCV 
- [What Is MCP, and Why Is Everyone – Suddenly!– Talking About It?](https://huggingface.co/blog/Kseniase/mcp) by Ksenia Se 
- [Model Context Protocol (MCP) an overview](https://www.philschmid.de/mcp-introduction) by Philipp Schmid 

## How it works?

### Core primitives (Official MCP Specification)

- [Resources](https://modelcontextprotocol.io/docs/concepts/resources)  
- [Prompts](https://modelcontextprotocol.io/docs/concepts/prompts) 
- [Tools](https://modelcontextprotocol.io/docs/concepts/tools) 

### Core concepts (Official MCP Specification)

- [Sampling](https://modelcontextprotocol.io/docs/concepts/sampling)  
- [Roots](https://modelcontextprotocol.io/docs/concepts/roots)  
- [Transports](https://modelcontextprotocol.io/docs/concepts/transports) 

### Main concepts

- [What Is the Model Context Protocol (MCP) and How It Works](https://www.descope.com/learn/post/mcp) by Descope   
- [Core architecture (Official MCP Specification)](https://modelcontextprotocol.io/docs/concepts/architecture) 
- [MCP – A Beginner’s Guide](https://opencv.org/blog/model-context-protocol/#h-features-of-mcp-server) by sandeep, OpenCV  
- [MCP architecture and core components](https://www.descope.com/learn/post/mcp#mcp-architecture-and-core-components) by Descope 
- [How MCP works](https://www.descope.com/learn/post/mcp#how-mcp-works) by Descope (very nice and details protocol handshake sequential diagram)

## Best practices (Official MCP Specification)

- [Architecture](https://modelcontextprotocol.io/docs/concepts/architecture#best-practices) 
- [Resources](https://modelcontextprotocol.io/docs/concepts/resources#best-practices) 
- [Prompts](https://modelcontextprotocol.io/docs/concepts/prompts#best-practices) 
- [Tools](https://modelcontextprotocol.io/docs/concepts/tools#best-practices) 
- [Tool annotations](https://modelcontextprotocol.io/docs/concepts/tools#best-practices-for-tool-annotations) 
- [Sampling](https://modelcontextprotocol.io/docs/concepts/sampling#best-practices) 
- [Roots](https://modelcontextprotocol.io/docs/concepts/roots#best-practices) 
- [Transports](https://modelcontextprotocol.io/docs/concepts/transports#best-practices) 

## Security 

- [Security considerations for MCP servers](https://www.descope.com/learn/post/mcp#security-considerations-for-mcp-servers) by Descope   
- [Diving Into the MCP Authorization Specification](https://www.descope.com/blog/post/mcp-auth-spec) by Descope 
- [What about Security, Updates, Authentication?](https://www.philschmid.de/mcp-introduction#what-about-security-updates-authentication) by Philipp Schmid  

### Security considerations (Official MCP Specification)

- [Architecture](https://modelcontextprotocol.io/docs/concepts/architecture#security-considerations) 
- [Resources](https://modelcontextprotocol.io/docs/concepts/resources#security-considerations) 
- [Prompts](https://modelcontextprotocol.io/docs/concepts/prompts#security-considerations) 
- [Tools](https://modelcontextprotocol.io/docs/concepts/tools#security-considerations) 
- [Sampling](https://modelcontextprotocol.io/docs/concepts/sampling#security-considerations) 
- [Transports](https://modelcontextprotocol.io/docs/concepts/transports#security-considerations) 
- [Transport DNS Rebinding Attacks](https://modelcontextprotocol.io/docs/concepts/transports#security-warning%3A-dns-rebinding-attacks) 

## Use cases

- [MCP in Action](https://www.pulsemcp.com/use-cases) by PulseMCP 
- [MCP Server Use Cases](https://mcp.so/usercases) by MCP.so 
- [How We're Using MCP to Automate Real Workflows: 6 Working Use Cases](https://runbear.io/posts/How-Were-Using-MCP-to-Automate-Real-Workflows-6-Working-Use-Cases) by Joel Lim, Runbear 
- [Real-World Implementations: How Are Businesses Using MCP?](https://tldv.io/blog/model-context-protocol/#elementor-toc__heading-anchor-48) by Matt Leppington 
- [Use Cases](https://opencv.org/blog/model-context-protocol/#h-use-cases) by sandeep, OpenCV

## Interesting reads and references

- [Model Context Protocol Servers (Official MCP HitHub repository)](https://github.com/modelcontextprotocol/servers)
- [Example Servers (Official MCP Specification)](https://modelcontextprotocol.io/examples)
- [LLM isolation & the NxM problem](https://www.descope.com/learn/post/mcp#llm-isolation-&-the-nxm-problem) by Descope   
- [What is the Language Server Protocol?](https://microsoft.github.io/language-server-protocol/overviews/lsp/overview/) by Microsoft 
- [Humans in the Loop: The Design of Interactive AI Systems](https://hai.stanford.edu/news/humans-loop-design-interactive-ai-systems) by Ge Wang, Stanford HAI  
- [JSON-RPC 2.0 Specification](https://www.jsonrpc.org/specification) 
- [URI Template Specification](https://datatracker.ietf.org/doc/html/rfc6570) 
- [A collection of MCP servers](https://github.com/punkpeye/awesome-mcp-servers) by punkpeye (Frank Fiegel) 
- [A curated list of Model Context Protocol (MCP) servers](https://github.com/wong2/awesome-mcp-servers) by wong2 
- [TOP 11 Essential MCP Libraries](https://huggingface.co/blog/LLMhacker/top-11-essential-mcp-libraries) by JanusAI.Pro  
- [STDIO (Standard Input/Output)](https://en.wikipedia.org/wiki/Standard_streams)
- [Standard input, standard output, and standard error files](https://www.ibm.com/docs/en/aix/7.2?topic=redirection-standard-input-standard-output-standard-error-files) by IBM 
- [How Server-Sent Events (SSE) Work](https://dev.to/zacharylee/how-server-sent-events-sse-work-450a) by Zachary Lee 
- [The Model Context Protocol specification and SDKs (Official GitHub repo)](https://github.com/modelcontextprotocol)