# Changelog

All notable changes to the Logseq DB Knowledge Skill will be documented in this file.

## [1.0.1] - 2025-10-22

### Fixed
- **Critical Fix**: Corrected tag matching guidance in queries
  - Added new section "Tag Matching: Built-in vs Custom Tags" explaining the difference between `:db/ident` and `:block/title`
  - Updated all query examples to use `:block/title` (universal method that works for both built-in and custom tags)
  - Fixed common misconception that `:db/ident` works for custom user tags
  - Added examples showing the wrong approach (using `:db/ident` for custom tags like `#zotero`)
  - Updated README.md query examples to use `:block/title`

### Changed
- Recommended `:block/title` as the default tag matching method for better reliability
- Updated best practices section to reflect universal tag matching approach

## [1.0.0] - 2024-10-19

### Added
- Initial release of Logseq DB Knowledge Skill
- Comprehensive SKILL.md with core Logseq DB concepts
- Complete data model reference (data-model.md)
- Query examples cookbook with 50+ examples (query-examples.md)
- Side-by-side comparison guide (db-vs-file.md)
- MIT License
- README with installation and usage instructions

### Key Features
- Correct query syntax for Logseq DB (`{:query ...}` format)
- Task system explanation (#Task tags + Status properties)
- New tags (classes/supertags) documentation
- Property system (typed properties, tag properties, inheritance)
- Common query patterns and debugging tips
- Built-in features coverage (Journals, Cards, Templates, Assets)
