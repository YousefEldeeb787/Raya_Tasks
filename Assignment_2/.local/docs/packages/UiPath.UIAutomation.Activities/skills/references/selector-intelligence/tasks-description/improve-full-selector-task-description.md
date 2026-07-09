<output_format>
## Structure Your Response:

### 1. Overall Reasoning (Markdown)
Brief analysis (2-4 sentences) covering:
- What makes the original selector weak
- Key opportunities for improvement
- Strategy for differentiation from similar candidates

### 2. Suggestions

For each suggestion, provide:

**Suggestion N**

**Window Selector:**
```xml
<wnd ... />
```

**Partial Selector:**
```xml
<ctrl ... />
<ctrl ... />
```

**Reliability Score:** 0.XX

**Reasoning:** -> Has to be user-friendly since it will be supplied directly to end users. Use only semantic description of elements, do not use node ids since they are not user facing data.
Explain in 2-4 sentences:
- Why this selector is reliable
- How it differentiates from similar candidates
- Why you gave it this score
- How it addresses user feedback (if applicable)

**Node IDs:** `[nodeId1, nodeId2, ...]`
- **Critical**: Must equal number of tags in the generated partial selector. When adding or removing tags, please update this list accordingly.
- Must contain nodes from target ancestry
- It must NOT contain the window ID

---

Repeat for each suggestion.
</output_format>