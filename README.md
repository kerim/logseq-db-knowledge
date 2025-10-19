# Logseq DB Knowledge Skill for Claude

A comprehensive knowledge skill that teaches Claude AI the correct way to work with Logseq DB (database) graphs.

## Why This Skill Exists

Many AI models, including Claude, have embedded knowledge about the older **file-based version of Logseq**, which uses fundamentally different syntax and concepts. When working with **Logseq DB graphs**, this outdated knowledge leads to:

- ❌ Incorrect query syntax (missing `{:query ...}` wrapper)
- ❌ Wrong task format (using TODO markers instead of `#Task` tags)
- ❌ Misunderstanding of properties, tags, and data model

This skill corrects these misconceptions and ensures Claude provides accurate, working code for Logseq DB.

## What You'll Learn

### Core Concepts

- **Nodes**: How pages and blocks work as unified "nodes" in Logseq DB
- **New Tags** (Classes/Supertags): Structured tags with properties and inheritance
- **Properties**: Typed, validated properties (Text, Number, Date, DateTime, Checkbox, URL, Node)
- **Tasks**: How tasks actually work in DB (no TODO markers!)
- **Queries**: The correct query syntax for Logseq DB

### Critical Differences from File-Based Logseq

| Aspect | File-Based | Logseq DB |
|--------|------------|-----------|
| **Tasks** | `- TODO Task` | Block with `#Task` tag + Status property |
| **Queries** | `{{query (task TODO)}}` | `{:query [:find ...]}` |
| **Properties** | Untyped strings | Typed + validated |
| **Tags** | Simple labels | Classes with structure + inheritance |

## Installation

### For Claude Code (CLI)

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/logseq-db-knowledge.git

# Copy to Claude skills directory
cp -r logseq-db-knowledge ~/.claude/skills/
```

### For Claude Desktop App

1. Download the [latest release](https://github.com/YOUR_USERNAME/logseq-db-knowledge/releases)
2. Open Claude Desktop
3. Go to Settings → Skills
4. Click "Import Skill"
5. Select the downloaded zip file

## Quick Start

### Query Format (Most Important!)

**Every query in Logseq DB MUST use this format:**

```clojure
{:query [:find (pull ?b [*])
         :where
         [?b :block/tags ?t]
         [?t :db/ident :logseq.class/Task]]}
```

**Common Mistakes:**
```clojure
; ❌ WRONG - Old file-based syntax
{{query (task TODO)}}

; ❌ WRONG - Missing wrapper
[:find (pull ?b [*])
 :where ...]

; ✅ CORRECT
{:query [:find (pull ?b [*])
         :where ...]}
```

### Task Format

**In Logseq DB, tasks are NOT markers:**

```
❌ OLD (file-based):
- TODO This is a task
- DOING Working on it

✅ NEW (Logseq DB):
Block tagged with #Task
Status property: Todo, Doing, Done (NOT markers!)
```

**Query for tasks:**
```clojure
{:query [:find (pull ?b [*])
         :where
         [?b :block/tags ?t]
         [?t :db/ident :logseq.class/Task]
         [?b :logseq.property/status ?s]
         [?s :block/title "Doing"]]}
```

## What's Included

```
logseq-db-knowledge/
├── SKILL.md                    # Main skill file (loaded by Claude)
├── README.md                   # This file
└── references/
    ├── data-model.md          # Technical data model reference
    ├── query-examples.md      # 50+ query examples cookbook
    └── db-vs-file.md          # Side-by-side comparison guide
```

### Files Overview

- **SKILL.md**: Core knowledge about Logseq DB - nodes, tags, properties, tasks, queries
- **data-model.md**: Deep technical reference for the database schema and attributes
- **query-examples.md**: Complete cookbook with 50+ ready-to-use query examples
- **db-vs-file.md**: Comprehensive comparison between DB and file-based Logseq

## Key Topics Covered

### Nodes & Structure
- Understanding nodes (unified pages and blocks)
- Block hierarchy and relationships
- Page uniqueness by tag

### New Tags (Classes)
- Creating and configuring tags
- Tag properties and inheritance
- Parent tags via `Extends`

### Properties System
- Property types (Text, Number, Date, DateTime, Checkbox, URL, Node)
- Property configuration (defaults, choices, UI position)
- Tag properties (inherited by all tagged nodes)

### Tasks in Logseq DB
- Creating tasks with `#Task` tag
- Status property (Backlog, Todo, Doing, In Review, Done, Canceled)
- Task shortcuts and workflows
- Repeated tasks

### Queries
- Correct query wrapper format: `{:query ...}`
- Common query patterns (find by tag, status, property, content)
- Aggregations, sorting, filtering
- Debugging tips

### Built-in Features
- Journals (`#Journal`)
- Flashcards (`#Card`)
- Templates (`#Template`)
- Assets (`#Asset`)
- Tables and views

## Usage Examples

### Find all tasks that are currently in progress:
```clojure
{:query [:find (pull ?b [*])
         :where
         [?b :block/tags ?t]
         [?t :db/ident :logseq.class/Task]
         [?b :logseq.property/status ?s]
         [?s :block/title "Doing"]]}
```

### Find blocks referencing a specific person:
```clojure
{:query [:find (pull ?b [*])
         :where
         [?person :block/title "Alice"]
         [?b :block/refs ?person]]}
```

### Find all nodes with a custom tag:
```clojure
{:query [:find (pull ?b [*])
         :where
         [?b :block/tags ?t]
         [?t :db/ident :logseq.class/Person]]}
```

See `references/query-examples.md` for 50+ more examples!

## Common Use Cases

This skill helps with:

- ✅ Writing correct Logseq DB queries
- ✅ Understanding the DB data model
- ✅ Creating custom workflows with tags and properties
- ✅ Migrating from file-based to DB graphs
- ✅ Building complex queries (aggregations, joins, filters)
- ✅ Debugging query errors
- ✅ Understanding Logseq DB's unique features

## Recommended Approach

**Instead of writing raw queries, use Logseq's Query Builder:**

1. Type `/Query` in a block
2. Use the visual interface to select tags and filters
3. Logseq generates the correct syntax automatically

This is the easiest way to avoid syntax errors!

## Contributing

Found an issue or have improvements? Contributions are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

## Resources

- [Logseq Documentation](https://docs.logseq.com)
- [Logseq DB Overview](https://docs.logseq.com/#/page/db%20version)
- [Datascript Database](https://github.com/tonsky/datascript)
- [Claude Code Documentation](https://docs.claude.com/claude-code)

## License

MIT License - see LICENSE file for details

## Acknowledgments

Created for the Logseq community to bridge the gap between file-based and DB graph knowledge.

Special thanks to the Logseq team for building an amazing tool!

## Support

If this skill helps you, consider:
- ⭐ Starring this repository
- 📢 Sharing with others who use Logseq DB
- 🐛 Reporting issues or suggesting improvements
- 💡 Contributing examples or documentation

---

**Remember:** When working with Logseq DB, the query format is `{:query ...}` - not `{{query ...}}` and not bare `[:find ...]`!
