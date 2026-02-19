# AI Usage

**Owner**: Himanshu Dixit  
**Applies to**: All engineers

## The Rule

AI-assisted, human-approved. You're responsible for everything that ships.

## What We Use

- **GitHub Copilot** - Code completion, boilerplate
- **ChatGPT/Claude** - Architecture discussions, debugging
- **Cursor** - AI-powered IDE (optional)

## When AI Helps

- Boilerplate code
- Regex patterns
- SQL optimization
- Documentation drafts
- Test generation
- Debugging assistance

## When AI Doesn't Help

- Copy-paste without understanding
- Shipping AI code without review
- Security-critical code (auth, crypto)
- Bypassing learning

## Interns

**Month 1**: Learn fundamentals. Minimize AI.  
**Month 2**: Use AI for boilerplate. Explain what it does.  
**Month 3+**: Normal usage. You know the trade-offs.

## Security

**Don't**:
- Paste proprietary code into public AI tools
- Share secrets with AI
- trust AI-generated security code blindly

**Do**:
- Review everything AI suggests
- Run security scanners
- Get senior review for auth/crypto code

## Labeling

When you use AI, note it:

```python
# Generated with Copilot, reviewed by: Himanshu
def helper():
    pass
```

In PR description:
```
AI Usage:
- Copilot: helper functions
- ChatGPT: algorithm explanation
- All reviewed manually
```

---

himanshu.dixit@nexod.ai
