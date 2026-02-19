# AI Usage Policy

**Owner**: Himanshu Dixit  
**Applies to**: All team members  
**Effective**: Feb 2026

## Philosophy

**AI-assisted, human-approved.**

We use AI to accelerate development, not replace thinking. All AI-generated code must be:
- Understood by the author
- Reviewed by humans
- Tested before merge
- Documented appropriately

## Approved AI Tools

| Tool | Use Case | Notes |
|------|----------|-------|
| **GitHub Copilot** | Code completion | Daily driver |
| **ChatGPT/Claude** | Architecture discussions | Copy insights, not code |
| **Cursor** | AI-powered IDE | Optional replacement for VS Code |
| **Midjourney/DALL-E** | UI mockups | For inspiration only |

## What You CAN Use AI For

✅ **Allowed**:
- Boilerplate code generation
- Regex pattern writing
- SQL query optimization
- Documentation drafting
- Test case generation
- Explaining complex concepts
- Debugging assistance
- Code review comments

## What You CANNOT Use AI For

❌ **Prohibited**:
- Copy-pasting AI code without understanding
- AI-generated code in production without review
- Using AI for security-critical code (auth, crypto)
- AI-generated commit messages without context
- Passing off AI work as entirely your own in interviews
- Using AI to bypass learning (interns must understand fundamentals)

## Code Review Requirements

**AI-generated code must be labeled**:

```python
# Generated with Copilot, reviewed by: Himanshu
def complex_algorithm(data: list[int]) -> int:
    # AI suggested this optimization
    # Manually verified with test case X
    return sum(x for x in data if x > 0)
```

**PR Description**:
```markdown
## AI Usage
- Copilot used for: helper functions, test cases
- ChatGPT used for: algorithm explanation
- All code reviewed and tested manually
```

## Intern-Specific Rules

**Learning Requirement**:
- Week 1-4: Minimize AI use, learn fundamentals
- Week 5-8: AI allowed for boilerplate, must explain generated code
- Week 9-12: Normal AI usage, focus on high-level decisions

**AI-Assisted ≠ AI-Dependent**:
- Can you explain this code line by line?
- Can you write it without AI?
- Can you debug it if it breaks?

If no → Learn first, AI later.

## Security Guidelines

**Never**:
- Paste proprietary code into public AI tools
- Share API keys, passwords, or secrets with AI
- Use AI for cryptography or authentication logic
- Trust AI-generated security code without expert review

**Always**:
- Review AI suggestions for security issues
- Run security scanners on AI code
- Have senior engineer review AI security code

## Documentation

**When AI writes docs**:
- Fact-check all technical details
- Update with current information
- Add context specific to our project
- Remove generic filler content

## Violations

**First offense**: Warning + training
**Second offense**: Restricted AI access
**Third offense**: Termination consideration

## Best Practices

1. **Understand before using**: Know what the code does
2. **Test everything**: AI makes mistakes
3. **Keep it simple**: If AI solution is complex, simplify
4. **Document decisions**: Why AI vs manual?
5. **Stay current**: AI tools evolve, keep learning

## Tool-Specific Guidelines

### GitHub Copilot
- Accept suggestions with Tab
- Review before committing
- Disable for sensitive files (.env, secrets)
- Use comments to guide suggestions

### ChatGPT/Claude
- Use for explanations, not production code
- Ask "why" not just "how"
- Compare multiple approaches
- Verify with documentation

### Cursor
- Great for refactoring
- Review all AI changes
- Use Cmd+K for targeted edits
- Trust but verify

## Questions?

Ask in #ai-usage channel or email himanshu.dixit@nexod.ai

---

*Policy reviewed: Monthly*  
*Next review: March 2026*
