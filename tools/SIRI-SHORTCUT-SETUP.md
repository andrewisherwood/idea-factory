# Idea Factory — Siri Shortcut Setup

**Trigger phrase:** "Idea Factory"
**What it does:** Dictate an idea → markdown file created in `inbox/` → committed to GitHub. Under 10 seconds.

---

## Prerequisites

### GitHub Personal Access Token (fine-grained)

1. Go to https://github.com/settings/tokens?type=beta
2. **Generate new token**
3. Name: `idea-factory-siri`
4. Expiration: 90 days (set a calendar reminder to rotate)
5. Repository access: **Only select repositories** → `andrewisherwood/idea-factory`
6. Permissions: **Contents** → Read and write
7. Generate and copy the token — you'll need it in step 8 below

---

## Build the Shortcut

Open the **Shortcuts** app on your iPhone. Create a new shortcut. Name it **Idea Factory** (this is your Siri trigger phrase).

Add these actions in order:

### 1. Ask for Input

- Type: **Text**
- Prompt: **What's the idea?**

### 2. Set Variable

- Variable name: `IdeaText`
- Value: **Provided Input**

### 3. Date

- Get **Current Date**

### 4. Format Date (for markdown)

- Date: **Current Date**
- Date Format: **Custom** → `yyyy-MM-dd`
- Set variable: `TodayDate`

### 5. Format Date (for filename)

- Date: **Current Date**
- Date Format: **Custom** → `yyyy-MM-dd-HHmm`
- Set variable: `FileSlug`

### 6. Text

Build the markdown body. Type this, inserting variables where shown:

```
# Siri Capture

**Date:** [TodayDate]
**Source:** Siri capture

## Raw Capture

[IdeaText]
```

Tap to insert the `TodayDate` and `IdeaText` variables inline.

### 7. Base64 Encode

- Input: **Text** from step 6
- Encode with: **Base64**
- Line breaks: **None**
- Set variable: `EncodedContent`

### 8. Get Contents of URL

This is the GitHub API call that creates the file and commits it.

- **URL:** Tap to build this with variables:
  ```
  https://api.github.com/repos/andrewisherwood/idea-factory/contents/inbox/[FileSlug].md
  ```
  Insert the `FileSlug` variable inline.

- **Method:** PUT

- **Headers:**
  | Key | Value |
  |-----|-------|
  | Authorization | `Bearer ghp_YOUR_TOKEN_HERE` |
  | Accept | `application/vnd.github+json` |

- **Request Body:** JSON
  | Key | Type | Value |
  |-----|------|-------|
  | message | Text | `Capture via Siri: [FileSlug]` (insert variable) |
  | content | Text | `[EncodedContent]` (insert variable) |

### 9. Show Notification

- Title: **Idea captured**
- Body: **[IdeaText]** (insert variable — confirms what was saved)

---

## Test It

1. Say **"Hey Siri, Idea Factory"**
2. Dictate a test idea
3. Check the repo — a new file should appear in `inbox/`
4. Verify the markdown renders correctly

## Tips

- **Dictation works great.** Siri transcribes, the shortcut captures. Don't worry about formatting — that's what scoping is for.
- **Conflicts:** The filename includes hours and minutes (`yyyy-MM-dd-HHmm`), so collisions are near-impossible unless you capture two ideas in the same minute.
- **Offline:** Won't work offline — it needs the GitHub API. If you need offline capture, a native app is the path (future scope).
- **Token rotation:** Fine-grained tokens expire. Set a reminder when you create it.
