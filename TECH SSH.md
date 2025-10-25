---
tags: 
- LI
- CLI
- TECH
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Technology MOC|Back to Technology MOC]] | [[TECH Linux index|Back to index]]

# Generating a new SSH key

##### This creates a new SSH key, using the provided email as a label.
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"

# If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

# Adding your SSH key to the ssh-agent
##### Start the ssh-agent in the background.
```bash
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

##### Add your SSH private key to the ssh-agent.
```bash
ssh-add ~/.ssh/id_ed25519
```