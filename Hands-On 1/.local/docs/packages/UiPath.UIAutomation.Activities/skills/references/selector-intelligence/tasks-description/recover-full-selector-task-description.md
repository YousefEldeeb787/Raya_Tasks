<output_format>
## Structure Your Response:

### 1. Candidate Analysis (Markdown)
Analyze each candidate element (2-3 sentences per candidate):
- What is its confidence score and what does it indicate?
- What are its key characteristics based on attributes?
- How well does it match the original selector's intent?
- Is it suitable for the element purpose?

### 2. Target Identification (Markdown)
State which candidate you identified as the most likely original target (2-4 sentences):
- **Identified Target Node ID:** [nodeId]
- **Confidence Score:** [score from the data]
- Why this candidate is the most likely original target (consider both confidence score and semantic analysis)
- What evidence supports this choice
- How the application likely changed to break the original selector

### 3. Overall Reasoning (Markdown)
Brief analysis (2-4 sentences) covering:
- What makes the original selector weak in the current context
- Key opportunities for improvement
- Strategy for creating robust selectors

### 4. Suggestions

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
- How it adapts to the application changes
- Why you gave it this score
- How it addresses the recovery scenario

**Node IDs:** `[nodeId1, nodeId2, ...]`
- **Critical**: Must equal number of tags in the generated partial selector. When adding or removing tags, please update this list accordingly.
- Must contain nodes from the identified target's ancestry
- It must NOT contain the window ID
- First node must be the identified target's direct descendant from window

---

Repeat for each suggestion.
</output_format>