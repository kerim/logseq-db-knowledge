# Logseq DB Knowledge Skill

A comprehensive knowledge skill for Claude to ensure accurate understanding of Logseq DB (database) graphs.

## Purpose

This skill corrects common misconceptions when working with Logseq DB. Many AI models (including Claude) have embedded knowledge about the older file-based version of Logseq, which does **not** apply to Logseq DB graphs.

## What This Skill Provides

### Core Knowledge

- **Nodes**: Understanding that pages and blocks are both "nodes" with similar behavior
- **New Tags** (Classes/Supertags): Structured tags with properties and inheritance
- **Properties**: Typed, validated properties (Text, Number, Date, DateTime, Checkbox, URL, Node)
- **Tasks**: How tasks work differently in DB (no TODO markers, use #Task tag + Status property)
- **Queries**: Correct Datalog query syntax for Logseq DB

### Reference Documentation

1. **data-model.md**: Technical deep-dive into the Logseq DB data model
   - Entity-Attribute-Value structure
   - Database attributes and namespaces
   - Property storage and types
   - Task, journal, and query entities

2. **query-examples.md**: Complete query cookbook
   - 50+ ready-to-use query examples
   - Task queries (by status, priority, deadline)
   - Property queries (all types)
   - Aggregation, sorting, and advanced patterns
   - Common pitfalls and debugging tips

3. **db-vs-file.md**: Side-by-side comparison
   - Feature comparison table
   - Migration guidance
   - When to use which version
   - Common pitfalls when switching

## When to Use This Skill

Invoke this skill whenever:
- User mentions Logseq DB or database graphs
- Working with Logseq queries that don't return expected results
- User asks about tasks, properties, or tags in Logseq
- Debugging Logseq CLI or API queries
- Converting from file-based to DB graphs
- Creating custom workflows with new tags

## Installation

### For Claude Code (CLI)

Copy to your skills directory:
```bash
cp -r logseq-db-knowledge ~/.claude/skills/
```

### For Claude Desktop App

1. Download `logseq-db-knowledge.zip`
2. Open Claude Desktop
3. Go to Settings → Skills
4. Click "Import Skill"
5. Select the zip file
6. Done! The skill is now available

## Key Concepts

### Tasks Are Different!

**Old Way (File-based - DON'T USE):**
```markdown
- TODO This is a task
- DOING Working on this
```

**New Way (Logseq DB):**
```
Block tagged with #Task
Status property: Todo, Doing, Done (no markers!)
```

### Tags Are Structured

**Old Way:** `#research` is just a label

**New Way:** `#Research` is a class with:
- Properties (inherited by all tagged nodes)
- Parent tags (inheritance via Extends)
- Auto-applied templates

### Properties Are Typed

**Old Way:** All properties are strings

**New Way:** Properties have types:
- Number (sorts correctly as numbers)
- Date (links to journals, date picker)
- Node (type-safe references)
- Checkbox, URL, Text, DateTime

### Queries Must Change

**Old query (file-based):**
```clojure
[:find (pull ?b [*])
 :where
 [?b :block/marker "TODO"]]
```

**New query (Logseq DB):**
```clojure
[:find (pull ?b [*])
 :where
 [?b :block/tags ?t]
 [?t :db/ident :logseq.class/Task]
 [?b :logseq.property/status ?s]
 [?s :block/title "Todo"]]
```

## Quick Reference

### Common Attributes

**Node Attributes:**
- `:block/title` - Node title
- `:block/content` - Block content
- `:block/tags` - New tags on node
- `:block/refs` - Referenced nodes
- `:block/uuid` - Unique ID

**Property Namespaces:**
- `:logseq.property/status`
- `:logseq.property/priority`
- `:logseq.property/deadline`
- `:logseq.property/YOUR-PROPERTY-NAME`

**Tag Classes:**
- `:logseq.class/Task`
- `:logseq.class/Journal`
- `:logseq.class/Query`
- `:logseq.class/Card`
- `:logseq.class/YourTagName`

### Common Query Patterns

**Find all tasks:**
```clojure
[:find (pull ?b [*])
 :where
 [?b :block/tags ?t]
 [?t :db/ident :logseq.class/Task]]
```

**Find tasks by status:**
```clojure
[:find (pull ?b [*])
 :where
 [?b :block/tags ?t]
 [?t :db/ident :logseq.class/Task]
 [?b :logseq.property/status ?s]
 [?s :block/title "Doing"]]
```

**Find nodes with property:**
```clojure
[:find (pull ?b [*])
 :where
 [?b :logseq.property/email ?email]]
```

See `references/query-examples.md` for 50+ more examples!

## Files in This Skill

```
logseq-db-knowledge/
├── SKILL.md                    # Main skill file (loaded by Claude)
├── README.md                   # This file
└── references/
    ├── data-model.md          # Technical data model reference
    ├── query-examples.md      # Complete query cookbook
    └── db-vs-file.md          # Comparison guide
```

## Contributing

Found an issue or have suggestions? This skill was created to help users avoid frustration with Logseq DB. Feedback welcome!

## Resources

- [Logseq Documentation](https://docs.logseq.com)
- [Logseq DB Overview](https://docs.logseq.com/#/page/db%20version)
- [Datascript Database](https://github.com/tonsky/datascript)
- [Logseq CLI](https://www.npmjs.com/package/@logseq/cli)

## License

Created for the Logseq community. Use freely!

---

**Remember:** When working with Logseq DB, forget everything you know about file-based Logseq. Read the references in this skill to learn the new model!
