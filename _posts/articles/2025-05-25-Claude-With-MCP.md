---
title: "Claude with MCP"
categories:
  - articles
tags:
  - LLM
  - MCP
  - Production
  - Efficiency
excerpt: "Productivity"
toc: true
toc_sticky: true
toc_label: "Table of Contents"
---

In the past few weeks, I have been exploring MCP server with Claude desktop as a non-technical user. My goal is to integrate this into my daily workflow for productivity. In this short blog, I will share my experience and explain how I use it for my daily tasks.

## What is MCP?

MCP means Model Context Protocol. It has those components:

* Hosts are apps like Claude Desktop or Cursors that start the connection
* Clients connect to servers one-to-one inside the host app
* Servers give context, tools, and prompts to clients

![mcp_intro](/assets/images/articles/mcp.jpg){: .align-center width="380"}

For non-technical users, Claude desktop is the best choice for host to start with MCP. Clients can be understood as each LLM conversion chat. With MCP servers, Claude can work with your daily tools like Confluence, JIRA, and Google Drive without copying and pasting.

## Benefits of Using Claude with MCP

* Save Time: Get information fast without switching apps
* Better Workflow: Do tasks by talking to Claude
* Less Manual Work: Let Claude gather and organize information
* Better Decisions: See all your data in one place

## What Can Claude Do With MCP?

After trying more than 30 MCP services, I kept these servers in my Claude desktop. Here's how each one helps me work better:

![mcp_servers](/assets/images/articles/mcp_servers.png){: .align-center width="380"}

### Filesystem 

You can find the setup guide in Claude's [Blog](https://modelcontextprotocol.io/quickstart/user). It lets your LLM read, create, change, and organize files. For example, I ask it to clean up messy folders or save data from LLM outputs.

### Atlassian

This is an official [integration](https://www.anthropic.com/news/integrations) from Claude. It can search and summarize pages without opening them, find information across pages, and create new pages. It saves you from building RAG for Confluence. In JIRA, it can check ticket status, create new tickets, update tickets, and move tickets through your workflow.

### Slack 

This server is [here](https://github.com/modelcontextprotocol/servers/tree/main/src/slack). It lets Claude work with your Slack workspace. I use it to summarize messages from channels I don't check often. I also combine it with other MCP services. For example, I can send a message in Slack and create a JIRA ticket at the same time by typing in Claude desktop.

### Firecrawal 

This is a useful [integration](https://www.firecrawl.dev/mcp) that gets information from websites without writing code. I tried it for different cases. It can get fee structures and orderbook data from websites. The LLM can then organize this information into useful formats. You can also use the filesystem tools to save it locally.

### Langfuse

The Langfuse MCP [server](https://langfuse.com/docs/prompts/mcp-server) adds prompt management to Claude desktop. You can get saved prompts and organize the ones you're using. For example, I can type a Slack message and an assignee name, and the prompt will trigger both Slack and Atlassian MCP servers to send the message and create a JIRA ticket.

![lf](/assets/images/articles/langfuse.png){: .align-center width="380"}

### Spotify

This is another cool integration I use. It lets me change playlists without opening Spotify. I can also create playlists when I don't know what to listen to. For example, I always ask it to make a playlist based on my Slack messages from today.

## Thoughts on MCP

### Using Multiple MCP Servers

The best part is using multiple MCP services together. It creates better workflows and works like having agents without coding. For example, I just combined Langfuse, Slack and JIRA together which can automate the flow for Slack notification and JIRA creation easily. And you can bring more creativity for fun.

### Context Limits

You need to understand what each MCP service can do. If the search is too broad, it will be slow or hit context limits and fail. For example, the Slack MCP server can't search channels by name. If you just ask it to send a message to a channel name, it will fail. The better way is to list all key channel IDs in your Slack server setup. This ensures the server can find the right channel.

### Productivity

MCP can definitely bring productivity. In nature, MCP with LLM can make natural language driven workflow possible. However, to achieve more autonomous for complex workflow, it requires a deep understanding and view for the process end-to-end and also the sense of the capabilities of MCP servers and also LLM.