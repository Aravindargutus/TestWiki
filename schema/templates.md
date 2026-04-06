# Page Templates

## Source Page Template
```markdown
---
title: [Source Title]
type: source
created: YYYY-MM-DD
updated: YYYY-MM-DD
source_file: raw/[filename]
tags: []
---

# [Source Title]

## Summary
_2-3 paragraph summary of the source._

## Key Takeaways
- Takeaway 1
- Takeaway 2

## Entities Mentioned
- [[entity-name]] — context of mention

## Concepts Introduced
- [[concept-name]] — how it's discussed

## Open Questions
- Questions raised by this source
```

## Entity Page Template
```markdown
---
title: [Entity Name]
type: entity
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
tags: []
---

# [Entity Name]

## Overview
_Brief description of the entity._

## Key Facts
- Fact 1
- Fact 2

## Appearances
- [[source-page]] — context of mention

## Relationships
- Related to [[other-entity]] — nature of relationship

## Notes
_Additional observations._
```

## Concept Page Template
```markdown
---
title: [Concept Name]
type: concept
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
tags: []
---

# [Concept Name]

## Definition
_Clear definition of the concept._

## Key Aspects
- Aspect 1
- Aspect 2

## Sources
- [[source-page]] — how this source discusses the concept

## Related Concepts
- [[other-concept]] — nature of relationship

## Evolution
_How understanding of this concept has changed across sources._
```

## Comparison Page Template
```markdown
---
title: [A] vs [B]
type: comparison
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
tags: []
---

# [A] vs [B]

## Overview
_Why this comparison matters._

| Dimension | [A] | [B] |
|-----------|-----|-----|
| ... | ... | ... |

## Analysis
_Deeper discussion of the comparison._

## Sources
- [[source-page]]
```

## Analysis Page Template
```markdown
---
title: [Analysis Title]
type: analysis
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
tags: []
question: "[Original question that prompted this analysis]"
---

# [Analysis Title]

## Question
_The question that prompted this analysis._

## Answer
_Synthesized answer with citations._

## Sources Consulted
- [[wiki-page]] — what it contributed

## Follow-up Questions
- Questions this analysis raises
```
