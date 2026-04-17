## name: contentstack-workflow

description: |  
  Manages Contentstack workflows for content creation, publishing, and asset management.  
  Use when users mention "Contentstack", "create content entry", "publish content", "manage assets",  
  or ask to "update content" or "work with Contentstack entries".  
license: MIT  
metadata:  
  author: Rajat Goel  
  version: 1.0.0  
  category: content-management  
  tags: [contentstack, cms, content, publishing, automation]

# Contentstack Workflow Skill

## Instructions

### **Step 1: Determine User Intent**

- If the user mentions **Contentstack**, **content entry**, **publish**, or **asset management**, load this skill.
- Clarify the user's goal:
  - Are they creating, updating, or publishing content?
  - Are they managing assets (uploading, linking, or organizing)?
  - Do they need help with workflows (approvals, scheduling)?

### **Step 2: Content Creation**

- If the user wants to **create a content entry**:
  1. Ask for:
    - Content type (e.g., blog post, product page).
    - Title, body, and metadata (SEO, tags, categories).
    - Target environment (staging/production).
  2. Use Contentstack's API (via MCP) to:
    - Create a draft entry.
    - Populate fields based on user input.
    - Validate required fields (e.g., title, slug).
  3. Confirm with the user before proceeding.

### **Step 3: Content Publishing**

- If the user wants to **publish content**:
  1. Check if the entry exists (use MCP to fetch entry by ID/title).
  2. If found:
    - Ask for confirmation to publish.
    - Use MCP to publish to the specified environment.
    - Provide a preview link if available.
  3. If not found:
    - Offer to create a new entry (see Step 2).

### **Step 4: Asset Management**

- If the user wants to **manage assets**:
  1. For **uploads**:
    - Ask for the asset file (image, PDF, etc.).
    - Use MCP to upload to Contentstack.
    - Return the asset URL and metadata.
  2. For **linking assets to entries**:
    - Ask for the entry ID and asset ID.
    - Use MCP to update the entry with the asset reference.
  3. For **organizing assets**:
    - Ask for folder/collection details.
    - Use MCP to move or categorize assets.

### **Step 5: Workflow Automation**

- If the user mentions **workflows** (e.g., approvals, scheduling):
  1. Fetch the current workflow status (via MCP).
  2. Offer actions:
    - Submit for approval.
    - Schedule publish date/time.
    - Revert to draft.
  3. Use MCP to update the workflow status.

### **Step 6: Error Handling**

- If MCP calls fail:
  - Check for:
    - Invalid API keys or permissions.
    - Missing required fields.
    - Network issues.
  - Prompt the user to:
    - Recheck credentials (Settings > Extensions > Contentstack).
    - Provide missing details.
  - Retry the operation if safe.

### **Step 7: Provide Output**

- After completing the task:
  - Summarize actions taken.
  - Provide links to the published content or asset.
  - Offer next steps (e.g., "Would you like to create another entry?").

---

## Examples

### **Example 1: Create and Publish a Blog Post**

**User says:** *"Create a blog post in Contentstack titled 'AI in 2026' with the body 'The future is here...' and publish it to production."*

**Actions:**

1. Create a draft entry for "blog_post" content type.
2. Populate title, body, and metadata.
3. Publish to production.
4. Return: *"Blog post 'AI in 2026' published! [View here](#)."*

### **Example 2: Upload an Asset**

**User says:** *"Upload this image to Contentstack and link it to my 'Homepage' entry."*

**Actions:**

1. Upload the image via MCP.
2. Fetch the "Homepage" entry.
3. Update the entry to reference the new asset.
4. Return: *"Image uploaded and linked to 'Homepage'. Asset URL: [link](#)."*

---

## Troubleshooting

### **MCP Connection Issues**

- **Symptom:** Skill loads but MCP calls fail.
- **Solution:**
  1. Verify Contentstack MCP is connected in Claude settings.
  2. Check API key permissions in Contentstack.
  3. Test MCP directly: *"Use Contentstack MCP to list my content types."*

### **Skill Not Triggering**

- **Symptom:** Skill doesn’t load for Contentstack-related queries.
- **Solution:**
  - Revise the `description` to include more trigger phrases (e.g., "manage Contentstack entries").
  - Test with: *"When would you use the contentstack-workflow skill?"*

### **Instructions Not Followed**

- **Symptom:** Skill loads but steps are skipped.
- **Solution:**
  - Ensure instructions are **actionable** (e.g., "Use MCP to publish entry" vs. "Handle publishing").
  - Add validation steps (e.g., "CRITICAL: Confirm user intent before publishing.").
