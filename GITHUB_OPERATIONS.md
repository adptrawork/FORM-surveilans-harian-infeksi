# GitHub Operations Guide - FORM-surveilans-harian-infeksi

## Repository Information
- **Owner**: adptrawork
- **Repository**: FORM-surveilans-harian-infeksi
- **URL**: https://github.com/adptrawork/FORM-surveilans-harian-infeksi.git
- **Default Branch**: main (likely)

## Setup GitHub CLI
If you haven't authenticated yet, run:
```bash
gh auth login
# Choose: GitHub.com
# Log in with your browser
```

## Current Status Check

### 1. Check Repository Issues
```bash
gh issue list --state all --limit 20
```

### 2. Check Pull Requests
```bash
gh pr list --state all --limit 20
```

### 3. Check Recent CI Runs
```bash
gh run list --limit 10
```

### 4. Check Repository Settings
```bash
gh repo view adptrawork/FORM-surveilans-harian-infeksi
```

## Issue Triage Commands

### Search for potential duplicates
```bash
# Search for form-related issues
gh issue list --search "form surveilans" --state all --limit 10

# Search for infection-related issues
gh issue list --search "infeksi" --state all --limit 10
```

### Add labels to issues
```bash
# Mark as bug with high priority
gh issue edit <NUMBER> --add-label "bug,high-priority"

# Mark as enhancement
gh issue edit <NUMBER> --add-label "enhancement,medium-priority"

# Mark as good first issue
gh issue edit <NUMBER> --add-label "good-first-issue"
```

### Comment on issues
```bash
# For bugs needing more info
gh issue comment <NUMBER> --body "Terima kasih melaporkan. Bisakah Anda berikan langkah-langkah reproduksi?"

# For duplicates
gh issue comment <NUMBER> --body "Issue ini duplikat dari https://github.com/adptrawork/FORM-surveilans-harian-infeksi/issues/ORIGINAL-NUMBER. Silakan lihat issue asli."

# For stale issues
gh issue comment <NUMBER> --body "Masalah ini tidak aktif selama 14+ hari. Apakah masih relevan?"
gh issue edit <NUMBER> --add-label "stale"
```

## Pull Request Management

### Review PR Checklist
```bash
# Check CI status
gh pr checks <NUMBER>

# Check if mergeable
gh pr view <NUMBER> --json mergeable

# View PR details
gh pr view <NUMBER> --json title,body,author,updatedAt,files
```

### Comment on PRs
```bash
# For PRs needing review
gh pr comment <NUMBER> --body "Perlu review dari tim. Mohan periksa CI dan linting."

# For PRs with issues
gh pr comment <NUMBER> --body "Ada beberapa masalah yang perlu diperbaiki sebelum merge:"
```

### Stale PRs
```bash
# Find PRs with no recent activity (>7 days)
gh pr list --json number,title,updatedAt --jq '.[] | select(.updatedAt < "2026-04-10")'

# Comment on stale PR
gh pr comment <NUMBER> --body "PR ini tidak aktif selama 7+ hari. Masih ingin melanjutkan?"
```

## CI/CD Operations

### Check recent CI runs
```bash
# List recent runs
gh run list --limit 10

# View failed run logs
gh run view <RUN-ID> --log-failed

# Rerun failed workflow
gh run rerun <RUN-ID> --failed
```

## Release Management

### Prepare release
```bash
# Check merged PRs since last release
gh pr list --state merged --base main --search "merged:>2026-04-01"

# Create release
gh release create v1.0.0 --title "v1.0.0 - Form Surveilans Harian" --generate-notes

# Create pre-release
gh release create v1.1.0-rc1 --prerelease --title "v1.1.0 Release Candidate 1"
```

## Security Monitoring

### Check Dependabot alerts
```bash
gh api repos/adptrawork/FORM-surveilans-harian-infeksi/dependabot/alerts --jq '.[].security_advisory.summary'
```

### Check secret scanning
```bash
gh api repos/adptrawork/FORM-surveilans-harian-infeksi/secret-scanning/alerts --jq '.[].state'
```

## Quality Gate Checklist

Before completing any GitHub operations task, ensure:
- [ ] All issues have appropriate labels (bug, feature-request, priority)
- [ ] No PRs older than 7 days without review/comment
- [ ] CI failures have been investigated
- [ ] Releases include accurate changelogs
- [ ] Security alerts are acknowledged and tracked

## Current Repository Tasks for FORM-surveilans-harian-infeksi

### High Priority
1. **Review Open PRs** - Check if there are any pending PRs needing review
2. **Triage Issues** - Classify open issues and add appropriate labels
3. **Check CI Status** - Ensure recent CI runs are passing

### Medium Priority  
1. **Update Repository Description** - Add description about the form surveilans system
2. **Add Contributing Guidelines** - Create CONTRIBUTING.md
3. **Add License** - If missing, add appropriate license (MIT suggested)

### Low Priority
1. **Update README** - Add documentation about the form
2. **Add Wiki Pages** - Create user guides and developer documentation
3. **Enable GitHub Pages** - For documentation hosting

## Sample Issue Template

When creating new issues, use this template:
```markdown
## Jenis Masalah
- [ ] Bug
- [ ] Feature Request  
- [ ] Question
- [ ] Documentation

## Prioritas
- [ ] Critical
- [ ] High
- [ ] Medium
- [ ] Low

## Deskripsi
[Deskripsikan masalah dengan jelas]

## Langkah-langkah Reproduksi
1. [Langkah pertama]
2. [Langkah kedua]
3. [Hasil yang diharapkan]

## Lingkungan
- Browser: [Browser yang digunakan]
- Versi: [Versi aplikasi]
```