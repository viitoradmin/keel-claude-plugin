# Keel Context for Claude

Keel turns your company's documents into searchable, evidence-backed knowledge. This plugin connects Claude to your Keel workspace and teaches it to answer from your own documents, show the exact source passage behind each answer, list facts Keel has verified, and add new text, records or files to your workspace.

Claude only sees the documents your own Keel account can see. The first time you use it, you sign in to Keel and choose which workspaces Claude may use and whether it may add content.

## Requirements

A Keel account with access to at least one workspace.

## Install (Claude Code)

```
/plugin install keel-context --marketplace viitoradmin/keel-claude-plugin
```

## What's included

- The Keel connector (https://api-keel.viitor.ai/mcp)
- A skill that tells Claude when to search Keel, how to cite sources, and when it may add content
- Two agents for RFP and proposal work (Cowork and Claude Code):
  - `keel-rfp-writer`: writes a new RFP for your project from its requirements in Keel, and saves it after your approval
  - `keel-proposal-writer`: writes your proposal in reply to a client's RFP from Keel evidence, and saves it after your approval

## Example prompts

- Using Keel, which certifications does our company hold, and which document confirms each one?
- Using Keel, create an RFP for the field-service app project.
- Using Keel, find past projects that match the Example Logistics RFP.
- Using Keel, write our proposal for the Example Logistics RFP.
- Save these meeting notes to Keel.

## Links

- Documentation: https://keel.viitor.ai/docs/claude
- Privacy policy: https://keel.viitor.ai/privacy
- Support: support@keelcontext.com
