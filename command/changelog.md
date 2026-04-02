---
description: Generate changelog from git history
agent: release-helper
temperature: 0.2
---

Generate a comprehensive changelog from git history.

**Git Analysis:**
!`git log --oneline --since="[date]" --until="[date]" | head -50`

!`git log --format="%H %s" --grep="feat\|fix\|docs" -i | head -20`

!`git tag --list | tail -5`

## Changelog Generation

### Changelog Information
- **Version**: [version]
- **Date**: [date]
- **From Commit**: [commit hash]
- **To Commit**: [commit hash]
- **Total Commits**: [count]

---

## Changelog Format

### Recommended Format (Keep a Changelog)
```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [Version] - [Date]

### Added
- Feature that was added
- Another feature

### Changed
- Change that was made
- Another change

### Deprecated
- Feature that will be removed
- Another deprecation

### Removed
- Feature that was removed
- Another removal

### Fixed
- Bug that was fixed
- Another bug fix

### Security
- Security improvement
- Vulnerability fix
```

---

## Generated Changelog

## [Unreleased] - [Current Date]

### Added
- [Feature]: [Description]
- [Feature]: [Description]

### Changed
- [Change]: [Description]
- [Change]: [Description]

### Deprecated
- [Feature]: [Description]
- [Deprecation]: [Description]

### Removed
- [Feature]: [Description]
- [Removal]: [Description]

### Fixed
- [Bug]: [Description]
- [Fix]: [Description]

### Security
- [Security]: [Description]
- [Vulnerability]: [Description]

---

## Detailed Changelog by Category

### 🚀 Features
| Commit | Author | Description |
|--------|--------|-------------|
| [hash] | [author] | [description] |
| [hash] | [author] | [description] |

### 🐛 Bug Fixes
| Commit | Author | Description |
|--------|--------|-------------|
| [hash] | [author] | [description] |
| [hash] | [author] | [description] |

### 📚 Documentation
| Commit | Author | Description |
|--------|--------|-------------|
| [hash] | [author] | [description] |
| [hash] | [author] | [description] |

### ♻️ Refactoring
| Commit | Author | Description |
|--------|--------|-------------|
| [hash] | [author] | [description] |
| [hash] | [author] | [description] |

### 💄 Styling
| Commit | Author | Description |
|--------|--------|-------------|
| [hash] | [author] | [description] |
| [hash] | [author] | [description] |

### ⚡ Performance
| Commit | Author | Description |
|--------|--------|-------------|
| [hash] | [author] | [description] |
| [hash] | [author] | [description] |

### 🔧 Maintenance
| Commit | Author | Description |
|--------|--------|-------------|
| [hash] | [author] | [description] |
| [hash] | [author] | [description] |

### 🧪 Testing
| Commit | Author | Description |
|--------|--------|-------------|
| [hash] | [author] | [description] |
| [hash] | [author] | [description] |

---

## Commits by Author

### [Author 1]
- [Commit]: [description]
- [Commit]: [description]
- [Commit]: [description]

### [Author 2]
- [Commit]: [description]
- [Commit]: [description]
- [Commit]: [description]

---

## Detailed Commit List

### Commit History
| Hash | Date | Author | Type | Description |
|------|------|--------|------|-------------|
| [hash] | [date] | [author] | [type] | [description] |
| [hash] | [date] | [author] | [type] | [description] |
| [hash] | [date] | [author] | [type] | [description] |

---

## Breaking Changes

### Breaking Change #1
**Commit**: [hash]
**Author**: [author]
**Description**: [description]

**Migration Guide**:
```bash
# Steps to migrate
[command 1]
[command 2]
[command 3]
```

### Breaking Change #2
**Commit**: [hash]
**Author**: [author]
**Description**: [description]

**Migration Guide**:
```bash
# Steps to migrate
[command 1]
[command 2]
[command 3]
```

---

## Security Updates

### Vulnerability #1
**Severity**: [Critical/High/Medium/Low]
**CVE**: [CVE-XXXX-XXXXX]
**Affected Version**: [version]
**Fixed in Version**: [version]
**Description**: [description]

### Vulnerability #2
**Severity**: [Critical/High/Medium/Low]
**CVE**: [CVE-XXXX-XXXXX]
**Affected Version**: [version]
**Fixed in Version**: [version]
**Description**: [description]

---

## Statistics

### Summary
- **Total Commits**: [count]
- **Contributors**: [count]
- **Features Added**: [count]
- **Bugs Fixed**: [count]
- **Breaking Changes**: [count]

### Changes by Type
| Type | Count | Percentage |
|------|-------|-------------|
| feat | [count] | [X%] |
| fix | [count] | [X%] |
| docs | [count] | [X%] |
| refactor | [count] | [X%] |
| style | [count] | [X%] |
| test | [count] | [X%] |
| chore | [count] | [X%] |

---

## Comparison with Previous Version

### New Features
1. [Feature 1]: [description]
2. [Feature 2]: [description]

### Improvements
1. [Improvement 1]: [description]
2. [Improvement 2]: [description]

### Bug Fixes
1. [Bug Fix 1]: [description]
2. [Bug Fix 2]: [description]

### Known Issues
1. [Issue 1]: [description and workaround]
2. [Issue 2]: [description and workaround]

---

## Upgrade Guide

### From Version X to Version Y

#### Breaking Changes
- [Change 1]: Migration steps
- [Change 2]: Migration steps

#### New Requirements
- [Requirement 1]
- [Requirement 2]

#### Deprecation Warnings
- [Deprecated feature]: Will be removed in [version]

#### Recommended Upgrade Steps
```bash
# Step 1: Backup
[backup command]

# Step 2: Update dependencies
[update command]

# Step 3: Run migrations
[migration command]

# Step 4: Clear cache
[cache command]

# Step 5: Verify
[verification command]
```

---

## Full Commit Log

```
[commit history]
```

---

## Generate Changelog Command

To generate a changelog automatically:

```bash
# Using git-changelog
npx git-changelog -o CHANGELOG.md

# Using conventional-changelog
npx conventional-changelog -p angular -i CHANGELOG.md -s

# Manual generation
git log --oneline --all --decorate --graph > CHANGELOG.md
```

---

## Automation Setup

### GitHub Actions Workflow
```yaml
name: Changelog

on:
  release:
    types: [created]

jobs:
  changelog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Generate Changelog
        run: |
          git log $(git describe --tags --abbrev=0 ^)..HEAD --pretty=format:"%h %s" > CHANGELOG.md
      - name: Upload Release Asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ github.event.release.upload_url }}
          asset_path: CHANGELOG.md
          asset_name: CHANGELOG.md
          asset_content_type: text/markdown
```

---

## Best Practices

### Changelog Guidelines
1. **Write for users, not developers**
2. **Use clear, concise language**
3. **Group by category**
4. **Use consistent formatting**
5. **Reference issues and PRs**
6. **Include migration guides for breaking changes**

### What to Include
- ✅ New features
- ✅ Bug fixes
- ✅ Security updates
- ✅ Breaking changes
- ✅ Deprecations
- ✅ Important changes

### What to Exclude
- ❌ Routine maintenance
- ❌ Minor documentation fixes
- ❌ Internal refactoring
- ❌ Temporary features
- ❌ Experimental features

---

## Release Checklist

- [ ] All commits categorized
- [ ] Breaking changes documented
- [ ] Migration guide created
- [ ] Version number updated
- [ ] Tag created
- [ ] Release notes written
- [ ] Changelog updated
- [ ] Announcement prepared
