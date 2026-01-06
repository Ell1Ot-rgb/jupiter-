# Migration Instructions: Omega21 Models and 5 Capas Documentation

**Last Updated:** 2026-01-06 UTC  
**Migration Source:** HIPERGRAFO Repository  
**Migration Target:** jupiter- Repository  
**Prepared by:** Ell1Ot-rgb

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Pre-Migration Checklist](#pre-migration-checklist)
4. [Migration Steps](#migration-steps)
5. [Post-Migration Verification](#post-migration-verification)
6. [Rollback Procedures](#rollback-procedures)
7. [Troubleshooting](#troubleshooting)

---

## Overview

This document provides detailed instructions for migrating:
- **Omega21 Calibrated Models** - Pre-trained machine learning models with calibration data
- **5 Capas Documentation** - Comprehensive documentation for the 5-layer (5 capas) architecture

### Migration Goals
- Preserve model integrity and calibration parameters
- Maintain documentation accuracy and structure
- Ensure proper directory organization in the jupiter- repository
- Maintain version history and metadata

---

## Prerequisites

### System Requirements
- Git >= 2.20
- Python >= 3.8 (if working with model files)
- Read/Write access to both HIPERGRAFO and jupiter- repositories
- Sufficient disk space (minimum 5GB recommended)
- Network connectivity for repository operations

### Required Permissions
- Write access to `Ell1Ot-rgb/jupiter-` repository
- Read access to `Ell1Ot-rgb/HIPERGRAFO` repository
- Administrative access to GitHub (for branch protection rule adjustments if needed)

### Tools & Configuration
```bash
# Verify Git installation
git --version

# Configure Git credentials (if not already done)
git config --global user.name "Ell1Ot-rgb"
git config --global user.email "your-email@domain.com"

# Clone both repositories
git clone https://github.com/Ell1Ot-rgb/HIPERGRAFO.git
git clone https://github.com/Ell1Ot-rgb/jupiter-.git
```

---

## Pre-Migration Checklist

Before starting the migration, complete the following steps:

### 1. Inventory Assessment
- [ ] Identify all Omega21 model files in HIPERGRAFO
  - Location: `HIPERGRAFO/models/omega21/`
  - File types: `.h5`, `.pkl`, `.pt`, `.pth`, `.joblib`
  - Check for calibration files: `*.cal`, `*.calib`, `calibration_data.json`

- [ ] List all 5 Capas documentation
  - Location: `HIPERGRAFO/docs/5_capas/`
  - File types: `.md`, `.rst`, `.pdf`, `.txt`
  - Check for supplementary files: images, diagrams, tables

### 2. Backup Creation
```bash
# Create backup directories locally
mkdir -p /backup/omega21_models
mkdir -p /backup/5_capas_docs

# Copy from HIPERGRAFO
cp -r HIPERGRAFO/models/omega21/* /backup/omega21_models/
cp -r HIPERGRAFO/docs/5_capas/* /backup/5_capas_docs/
```

### 3. Validation of Source Files
```bash
# Generate checksums for verification
cd /backup/omega21_models
find . -type f -exec sha256sum {} \; > ../omega21_checksums.txt

cd /backup/5_capas_docs
find . -type f -exec sha256sum {} \; > ../5_capas_checksums.txt
```

### 4. Documentation Review
- [ ] Review Omega21 model metadata and README
- [ ] Verify 5 Capas documentation completeness
- [ ] Identify any dependencies or external references
- [ ] Check for sensitive information that shouldn't be migrated

---

## Migration Steps

### Step 1: Prepare the jupiter- Repository

```bash
# Navigate to jupiter- repository
cd jupiter-

# Create feature branch for migration
git checkout -b migration/omega21-5capas

# Create target directory structure
mkdir -p models/omega21/calibrated
mkdir -p docs/5_capas
mkdir -p docs/5_capas/images
mkdir -p docs/5_capas/supplementary

# Verify directory creation
git status
```

### Step 2: Copy Omega21 Models

```bash
# Copy model files
cp -r /backup/omega21_models/* jupiter-/models/omega21/calibrated/

# Verify copy integrity
cd jupiter-/models/omega21/calibrated/
sha256sum -c /backup/omega21_checksums.txt

# Create a manifest file
cat > MODELS_MANIFEST.md << 'EOF'
# Omega21 Calibrated Models Manifest

## Models Included
- omega21_v1_0.h5 - Base model
- omega21_v1_0.pkl - Preprocessor
- omega21_calibration_data.json - Calibration parameters
- omega21_test_metrics.json - Performance metrics

## Calibration Details
- Calibrated: [DATE]
- Calibration Dataset: [DATASET_INFO]
- Performance Metrics: [METRICS_SUMMARY]

## Usage Instructions
See `docs/5_capas/OMEGA21_USAGE.md` for detailed usage guidelines.

EOF
```

### Step 3: Copy 5 Capas Documentation

```bash
# Copy documentation files
cp -r /backup/5_capas_docs/* jupiter-/docs/5_capas/

# Create main 5 Capas README
cat > jupiter-/docs/5_capas/README.md << 'EOF'
# 5 Capas (5 Layers) Architecture Documentation

## Overview
This directory contains comprehensive documentation for the 5-layer architecture implementation.

## Contents
- `ARCHITECTURE.md` - Detailed architecture specification
- `LAYER_DESCRIPTIONS.md` - Layer-by-layer breakdown
- `IMPLEMENTATION_GUIDE.md` - Implementation instructions
- `EXAMPLES/` - Usage examples and code samples
- `images/` - Diagrams and visual references
- `supplementary/` - Additional reference materials

## Quick Start
1. Review `ARCHITECTURE.md` for overview
2. Study `LAYER_DESCRIPTIONS.md` for layer details
3. Follow `IMPLEMENTATION_GUIDE.md` for implementation
4. Reference `EXAMPLES/` for code samples

## Related
- Omega21 Models: `models/omega21/`
- See `models/omega21/calibrated/MODELS_MANIFEST.md` for model details

EOF
```

### Step 4: Create Migration Documentation

```bash
# Create INDEX file for quick reference
cat > jupiter-/OMEGA21_5CAPAS_INDEX.md << 'EOF'
# Omega21 Models & 5 Capas - Quick Index

## Directory Structure
```
jupiter-/
├── models/
│   └── omega21/
│       ├── calibrated/
│       │   ├── omega21_v1_0.h5
│       │   ├── omega21_v1_0.pkl
│       │   ├── omega21_calibration_data.json
│       │   ├── omega21_test_metrics.json
│       │   └── MODELS_MANIFEST.md
│       └── README.md
├── docs/
│   └── 5_capas/
│       ├── README.md
│       ├── ARCHITECTURE.md
│       ├── LAYER_DESCRIPTIONS.md
│       ├── IMPLEMENTATION_GUIDE.md
│       ├── images/
│       └── supplementary/
└── OMEGA21_5CAPAS_INDEX.md (this file)
```

## Key Files
- **Models**: `models/omega21/calibrated/`
- **Documentation**: `docs/5_capas/`
- **Manifest**: `models/omega21/calibrated/MODELS_MANIFEST.md`

## Migration Completed
- Date: 2026-01-06
- Source: HIPERGRAFO
- By: Ell1Ot-rgb

EOF
```

### Step 5: Stage and Commit Changes

```bash
# Add all migrated files
git add models/omega21/
git add docs/5_capas/
git add OMEGA21_5CAPAS_INDEX.md

# Review staged changes
git status

# Create detailed commit
git commit -m "chore(migration): migrate Omega21 calibrated models and 5 capas documentation

- Move Omega21 v1.0 calibrated models from HIPERGRAFO
  - Include preprocessor, calibration data, and test metrics
  - Add MODELS_MANIFEST.md with metadata
  
- Move 5 Capas architecture documentation
  - Include architecture specifications and layer descriptions
  - Add implementation guides and examples
  - Maintain supplementary materials and diagrams
  
- Create index file for quick reference (OMEGA21_5CAPAS_INDEX.md)
- Create README files for proper documentation discovery

Migration source: HIPERGRAFO repository
Target: jupiter- repository
Migration date: 2026-01-06

Closes: migration/omega21-5capas"
```

### Step 6: Push to Remote Repository

```bash
# Push feature branch to remote
git push origin migration/omega21-5capas

# Verify push
git log --oneline -5
```

### Step 7: Create Pull Request

```bash
# Create pull request via GitHub CLI (if available)
gh pr create --title "Migration: Omega21 calibrated models and 5 capas documentation" \
  --body "This PR migrates Omega21 calibrated models and 5 capas documentation from HIPERGRAFO repository.

## Changes
- Omega21 v1.0 calibrated models with calibration data
- 5 Capas architecture documentation and layer specifications
- Manifests and index files for proper organization

## Migration Details
- Source: HIPERGRAFO
- Date: 2026-01-06
- Migrated by: Ell1Ot-rgb

## Verification
- All file checksums verified
- Directory structure validated
- Documentation completeness checked" \
  --base main
```

---

## Post-Migration Verification

### Step 1: File Integrity Verification

```bash
# Verify all files exist in jupiter-
cd jupiter-

# Check model files
ls -lah models/omega21/calibrated/

# Check documentation
ls -lah docs/5_capas/

# Verify checksums
cd models/omega21/calibrated/
sha256sum -c /backup/omega21_checksums.txt

cd ../../../docs/5_capas/
sha256sum -c /backup/5_capas_checksums.txt
```

### Step 2: Documentation Validation

```bash
# Check README files exist and are readable
cat models/omega21/calibrated/MODELS_MANIFEST.md
cat docs/5_capas/README.md
cat OMEGA21_5CAPAS_INDEX.md

# Verify no broken links in documentation (if using markdown link checker)
# markdownlint docs/5_capas/*.md
```

### Step 3: Model Files Validation

```bash
# For Python environments, verify model loading
python3 << 'EOF'
import os
import json

# Check JSON files are valid
model_dir = "models/omega21/calibrated"

json_files = [f for f in os.listdir(model_dir) if f.endswith('.json')]
for json_file in json_files:
    with open(os.path.join(model_dir, json_file), 'r') as f:
        try:
            data = json.load(f)
            print(f"✓ {json_file} is valid JSON")
        except json.JSONDecodeError as e:
            print(f"✗ {json_file} has JSON error: {e}")

# For model files, verify file integrity
model_files = [f for f in os.listdir(model_dir) if f.endswith(('.h5', '.pkl', '.pt', '.pth'))]
for model_file in model_files:
    filepath = os.path.join(model_dir, model_file)
    size = os.path.getsize(filepath)
    print(f"✓ {model_file}: {size} bytes")
EOF
```

### Step 4: Git History Verification

```bash
# Check commit history
git log --oneline migration/omega21-5capas -10

# Show file stats
git show --stat migration/omega21-5capas

# Verify merge commits (after merge)
git log --graph --oneline --all -20
```

### Verification Checklist

- [ ] All model files exist in `models/omega21/calibrated/`
- [ ] All documentation files exist in `docs/5_capas/`
- [ ] File checksums match original backups
- [ ] JSON files are valid and readable
- [ ] README and manifest files are present
- [ ] Git commits contain proper messages
- [ ] No merge conflicts
- [ ] Pull request passed all checks (CI/CD)

---

## Rollback Procedures

### Scenario 1: Rollback Before Merge

If issues are detected before merging to main:

```bash
# Delete the feature branch (local)
git branch -d migration/omega21-5capas

# Delete the feature branch (remote)
git push origin --delete migration/omega21-5capas

# Optionally, close the pull request without merging
# Use GitHub web interface to close PR
```

### Scenario 2: Rollback After Merge

If issues are detected after merging:

```bash
# Revert the merge commit
git log --oneline -10  # Find the merge commit SHA

git revert -m 1 <merge_commit_sha> --edit

# In the editor, provide revert reason:
# revert: migration commit due to [issue description]

git push origin main
```

### Scenario 3: Selective File Rollback

If only specific files need rollback:

```bash
# Identify problematic files
git diff HEAD~1 HEAD -- <filepath>

# Restore from backup
cp /backup/<original_file> <current_path>

# Commit the fix
git add <filepath>
git commit -m "fix(migration): correct issues in <filepath>"
git push origin main
```

### Scenario 4: Complete Repository Restore

If full rollback to HIPERGRAFO state is needed:

```bash
# Using backup
rm -rf models/omega21/calibrated
rm -rf docs/5_capas

# Restore from backup
cp -r /backup/omega21_models models/omega21/calibrated
cp -r /backup/5_capas_docs docs/5_capas

# Verify
sha256sum -c /backup/omega21_checksums.txt
sha256sum -c /backup/5_capas_checksums.txt

# Create rollback commit
git add models/ docs/
git commit -m "chore(rollback): restore original state before migration"
git push origin main
```

---

## Troubleshooting

### Issue 1: Checksum Mismatch

**Problem:** Files fail checksum verification after copy.

**Solution:**
```bash
# Recopy files carefully
cp -v /backup/<file> <destination>

# Regenerate checksums
sha256sum <destination>/* > new_checksums.txt

# Compare with original
diff /backup/checksums.txt new_checksums.txt

# If mismatch persists, verify source files in HIPERGRAFO
cd HIPERGRAFO
find models/omega21/ -type f -exec sha256sum {} \;
```

### Issue 2: Large File Handling

**Problem:** Git rejects files that are too large (>100MB).

**Solution:**
```bash
# Check file sizes
find models/omega21/calibrated -type f -exec du -h {} \;

# For very large files, consider Git LFS
git lfs install
git lfs track "*.h5"
git lfs track "*.pt"
git lfs track "*.pth"

# Add LFS files
git add .gitattributes
git add models/omega21/calibrated/
git commit -m "chore(migration): use Git LFS for large model files"
```

### Issue 3: Merge Conflicts

**Problem:** PR shows merge conflicts with main branch.

**Solution:**
```bash
# Update branch with latest main
git fetch origin
git rebase origin/main migration/omega21-5capas

# Resolve any conflicts manually
git status  # Shows conflicted files

# Edit conflicted files to resolve
# Then:
git add <resolved_files>
git rebase --continue

# Force push to update PR
git push origin migration/omega21-5capas --force-with-lease
```

### Issue 4: Documentation Links

**Problem:** Documentation links are broken after migration.

**Solution:**
```bash
# Update relative paths in documentation
# Example: ../../../models/omega21 → ../../models/omega21

# Use find and replace in editor
grep -r "HIPERGRAFO/models" docs/5_capas/
sed -i 's|HIPERGRAFO/models|../../../models|g' docs/5_capas/*.md

# Verify links are correct
grep -r "\.\./.*models" docs/5_capas/
```

### Issue 5: Permission Issues

**Problem:** Permission denied when accessing files.

**Solution:**
```bash
# Fix file permissions
chmod -R 755 models/omega21/
chmod -R 644 models/omega21/calibrated/*

# If it's a Git issue:
git config core.filemode false  # Ignore file mode changes

# Or fix on Windows:
icacls "models/omega21" /grant:r %USERNAME%:F /T
```

### Issue 6: CI/CD Pipeline Failures

**Problem:** Pull request fails GitHub Actions or CI checks.

**Solution:**
```bash
# Run local tests (if available)
# Check .github/workflows/ for test requirements

# View PR checks on GitHub:
# - Click "Details" on failed check
# - Review logs for specific errors

# Common fixes:
# - Ensure all files are properly committed
# - Check for syntax errors in documentation
# - Verify no sensitive information is exposed
# - Run linting: markdownlint, pylint, etc.

# After fixes:
git add <fixed_files>
git commit -m "fix(migration): address CI/CD pipeline issues"
git push origin migration/omega21-5capas
```

---

## Post-Migration Cleanup

### In HIPERGRAFO Repository

After successful migration and verification:

```bash
# Optional: Create migration reference
cat > HIPERGRAFO/MIGRATION_RECORD.md << 'EOF'
# Migration Record

## Omega21 Models & 5 Capas Migration

**Date:** 2026-01-06  
**Source:** HIPERGRAFO  
**Destination:** jupiter- repository  
**Status:** COMPLETED  
**Verified:** YES  

Files have been migrated to:
- `jupiter-/models/omega21/calibrated/`
- `jupiter-/docs/5_capas/`

Refer to `jupiter-/MIGRATION_INSTRUCTIONS.md` for details.

EOF
```

### Clean Up Backups

After migration is confirmed successful and in production for 1 week:

```bash
# Archive backups (keep for 30 days minimum)
tar -czf omega21_migration_backup_$(date +%Y%m%d).tar.gz \
  /backup/omega21_models \
  /backup/5_capas_docs \
  /backup/*.txt

# Store in secure location or archive storage

# Optional: Remove local backup after archival
rm -rf /backup/omega21_models /backup/5_capas_docs
```

---

## Success Criteria

The migration is considered **successful** when:

✓ All Omega21 model files are present in `jupiter-/models/omega21/calibrated/`  
✓ All 5 Capas documentation is present in `jupiter-/docs/5_capas/`  
✓ File checksums match originals  
✓ Documentation is readable and links are functional  
✓ Git history is clean and commit messages are descriptive  
✓ Pull request is merged to main without conflicts  
✓ No sensitive information is exposed  
✓ Backups are archived and secured  

---

## Support & Questions

For questions or issues during migration:

1. Review the **Troubleshooting** section above
2. Check commit messages: `git log --grep="migration"`
3. Review pull request comments and discussions
4. Contact: Ell1Ot-rgb
5. Reference this guide: `jupiter-/MIGRATION_INSTRUCTIONS.md`

---

## Appendix: Quick Command Reference

```bash
# Create branch
git checkout -b migration/omega21-5capas

# Copy files
cp -r /backup/omega21_models jupiter-/models/omega21/calibrated/
cp -r /backup/5_capas_docs jupiter-/docs/5_capas/

# Verify integrity
sha256sum -c /backup/omega21_checksums.txt

# Stage changes
git add models/omega21/ docs/5_capas/

# Commit
git commit -m "chore(migration): migrate Omega21 and 5 capas from HIPERGRAFO"

# Push
git push origin migration/omega21-5capas

# Create PR (GitHub CLI)
gh pr create --title "Migration: Omega21 and 5 capas documentation"

# Verify post-merge
git log --oneline -5
git show --stat
```

---

**Document Version:** 1.0  
**Last Updated:** 2026-01-06 UTC  
**Status:** READY FOR IMPLEMENTATION
