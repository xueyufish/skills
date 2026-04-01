---
name: xueyufish-translation
description: "Translation skill for markdown files - Chinese↔English bidirectional. IMPORTANT: Only activate when user explicitly requests translation. Do NOT auto-activate based on context."
---

# Universal Translation Skill

Translates markdown files between Chinese and English. Supports both translating specific files and scanning directories for files needing translation.

## What This Skill Does

This skill provides universal translation capabilities for markdown files:
- **Bidirectional translation**: Translate from Chinese to English or English to Chinese
- **Flexible input**: Translate individual files or scan directories for files needing translation
- **Content preservation**: Maintains original content while appending translations
- **Smart detection**: Uses language metadata to determine translation direction

## Activation

Activate this skill when the user explicitly requests translation of markdown files. Do NOT auto-activate based on context (e.g., when simply reading or editing a file).

---

## Workflow

### Step 1: Input Source Selection

Ask the user to choose translation mode:

**Option A: Translate specific file**
- User provides a file path
- Validate the file exists and is a `.md` file
- Proceed directly to Step 3

**Option B: Scan directory for files**
- User specifies a directory (default: current working directory)
- Scan recursively for markdown files (`.md` extension)
- Apply filter criteria below

**Filter criteria for directory scanning:**
- File must have YAML frontmatter with a `lang` attribute
- Determine translation direction based on `lang`:
  - `lang: CN` → translate to English
  - `lang: EN` (or other non-CN) → translate to Chinese
- Check `translated` attribute:
  - If `translated` attribute does NOT exist → include for translation
  - If `translated` is `false` → include for translation
  - If `translated` is `true` → skip (already translated)
- Skip files without `lang` attribute

### Step 2: Present Selection to User (Directory Mode Only)

If in directory scanning mode, present filtered files to the user for selection.

**Options format:**
- Present each article as a selectable option
- Show article title/relative path for identification
- Indicate translation direction (CN→EN or EN→CN)
- Allow multi-selection
- Include "Cancel" option to exit without processing

If in single file mode, skip this step and proceed directly to Step 3.

### Step 3: Translate Selected Articles

For each selected article:

1. **Determine translation direction**: Based on the `lang` attribute:
   - `lang: CN` → translate to English
   - `lang: EN` (or other non-CN) → translate to Chinese

2. **Preserve original content**: Keep all existing content intact

3. **Add translated content**: Append a new section after the original content:
   ```markdown
   # AI Translation

   [Translated content here - English if translating from CN, Chinese if translating from EN]

   ---
   *The above content is AI translated.*
   ```

4. **Update frontmatter**: Add or update the `translated` attribute to `true`

### Step 4: Report Results

After processing all selected articles, display a summary message:
- Number of articles successfully translated
- Confirmation to review the translated content

## YAML Frontmatter Structure

**Example 1: English to Chinese**
Input file:
```yaml
---
type: fleeting
lang: EN
status: wait
tags: [test]
---
```

Output file after translation:
```yaml
---
type: fleeting
lang: EN
status: wait
tags: [test]
translated: true
---

[Original English content...]

# AI Translation

[Translated Chinese content...]

---
*The above content is AI translated.*
```

**Example 2: Chinese to English**
Input file:
```yaml
---
type: fleeting
lang: CN
status: wait
tags: [test]
---
```

Output file after translation:
```yaml
---
type: fleeting
lang: CN
status: wait
tags: [test]
translated: true
---

[Original Chinese content...]

# AI Translation

[Translated English content...]

---
*The above content is AI translated.*
```

## Directory Context

- **Input modes**: Single file or directory scan
- **Default directory**: Current working directory (user can specify any directory)
- **File extensions**: `.md` only
- **Search scope**: Recursive through all subfolders (directory mode)

## Translation Notes

- Original content is never modified or deleted
- Translated content is appended to the end
- The `translated: true` flag prevents re-translation on subsequent runs
- Translation direction is automatically determined based on the `lang` attribute
