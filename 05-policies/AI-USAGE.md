# AI Usage Policy

**Owner**: Himanshu Dixit  
**Applies to**: All team members

## Rule

AI-assisted, human-approved.

## Approved Tools

| Tool | Use |
|------|-----|
| GitHub Copilot | Code completion |
| ChatGPT/Claude | Architecture, debugging |
| Cursor | AI IDE |

## Allowed

- Boilerplate code
- Regex patterns
- SQL optimization
- Documentation drafts
- Test generation
- Debugging help

## Not Allowed

- Copy-paste without understanding
- AI code in production without review
- Security-critical code (auth, crypto)
- Commit messages without context
- Bypassing learning (interns)

## Labeling

```python
# Generated with Copilot, reviewed by: Himanshu
def helper():
    pass
```

**PR description**:
```
AI Usage:
- Copilot: helper functions
- ChatGPT: algorithm explanation
- All reviewed manually
```

## Intern Rules

- Week 1-4: Minimize AI, learn fundamentals
- Week 5-8: AI for boilerplate, explain generated code
- Week 9-12: Normal usage

## Security

**Never**:
- Paste proprietary code into public AI
- Share secrets with AI
- Trust AI security code

**Always**:
- Review AI suggestions
- Run security scanners
- Senior review for security code

## Violations

1st: Warning + training  
2nd: Restricted AI access  
3rd: Termination

---

Questions: himanshu.dixit@nexod.ai
