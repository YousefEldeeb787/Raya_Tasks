## Java:

a) very reliable: role, cls, accessibleClass, hasTableAncestor
b) alright: name
c) specific for tables: rowName, colName, tableRow, tableCol
d) last resort, only use if needed to make the selector unique: virtualname, javastate, backgroundColor, foregroundColor

Semantic attributes like `name` and `role` are highly preferred.

CRITICAL: The `role` attribute is matched case-insensitive by default for java tags.