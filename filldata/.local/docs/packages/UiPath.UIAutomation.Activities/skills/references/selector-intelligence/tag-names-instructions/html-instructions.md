## Html:

Html tags are used to represent browser tabs.

a) very reliable: app, url
b) alright: title, htmlwindowname, mobiledata

CRITICAL: Do not change the attributes `app` or `mobiledata` if present on the HTML selectors since they are critical to identify the correct browser technology.

For browser tabs, prefer `url` over `title` when differentiating between tabs — URLs (especially domains) are far more stable than page titles. Use wildcards on URL paths (e.g., `url='https://example.com/*'`) to absorb navigation within a site while keeping the domain anchor.

Before wildcarding html attributes, scan the Window Level Context for other elements that would also match — if wildcarding causes collisions with other open tabs, use a more specific attribute or tighter wildcard.