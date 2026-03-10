
**Step 6 - Create:** `management/email-organizer-skill.md`

**Copy and paste this:**
```
# Email Organizer Skill

## Description
This skill enables AI agents to automatically organize, categorize, and manage email folders based on content, sender, and priority.

## Capabilities
- Automatically categorize incoming emails
- Create and manage folder structures
- Flag important emails for follow-up
- Archive old conversations
- Identify and handle spam
- Sort emails by project, client, or topic

## Categorization Rules

### By Sender
```python
def categorize_by_sender(email):
    """
    Categorize emails based on sender type
    
    Categories:
    - Clients: @client-domain.com, known client emails
    - Internal: @company.com, team members
    - Newsletter: mailchimp, substack, newsletter@
    - Spam: Suspicious patterns, blacklisted senders
    """
```

### By Content
```python
def categorize_by_content(email_body):
    """
    Analyze email content to determine category
    
    Categories:
    - Meeting Requests: Contains "meeting", "schedule", "calendar"
    - Invoices: Contains "invoice", "payment", "receipt"
    - Support: Contains "help", "issue", "problem", "bug"
    - Project Updates: Project names, status reports
    """
```

## Priority Scoring

### Priority Levels
| Level | Score | Action |
|-------|-------|--------|
| Urgent | 90-100 | Flag, notify immediately |
| High | 70-89 | Mark important, review soon |
| Medium | 40-69 | Read when available |
| Low | 10-39 | Batch process later |
| Spam | 0-9 | Auto-delete or spam folder |

### Priority Factors
- **Sender importance**: VIP clients, managers, team leads
- **Keywords**: "urgent", "deadline", "asap", "important"
- **Thread activity**: Multiple replies, participants
- **Time sensitivity**: Deadlines mentioned, event dates

## Folder Management

### Default Folders
```
Inbox/
├── Action Required/
├── Waiting For Reply/
├── To Read/
├── Client Communications/
├── Internal/
├── Projects/
│   ├── Project Alpha/
│   └── Project Beta/
└── Archive/
    ├── 2024/
    └── 2023/
```

### Auto-rules Examples
```python
# Client emails go to Client folder
if sender_domain in client_domains:
    move_to_folder("Clients/" + client_name)

# Project-specific emails
if "Project Alpha" in subject or body:
    move_to_folder("Projects/Project Alpha")

# Meeting invitations
if "invite" in content or "calendar" in content:
    move_to_folder("Meetings")
    flag_for_calendar()
```

## Smart Features

### Thread Detection
- Group related emails into conversations
- Track reply chains
- Identify latest message in thread

### Attachment Management
- Extract and save attachments to cloud storage
- Categorize by file type (PDFs, images, docs)
- Link to relevant projects

### Cleanup Automation
```python
def cleanup_old_emails():
    """
    Automatic cleanup rules
    """
    # Archive emails older than 1 year
    archive_emails(days=365)
    
    # Delete spam daily
    delete_spam(frequency="daily")
    
    # Clean social notifications after 30 days
    delete_category("Social", days=30)
```

## Example Workflow

**Input Email:**
```
From: client@company.com
Subject: Urgent: Project Alpha needs review
Body: We need final approval by tomorrow at 5pm
```

**AI Agent Processing:**
```
1. Analyze sender → client@company.com (High priority)
2. Detect keywords → "Urgent", "tomorrow", "5pm" (Urgent priority)
3. Identify topic → "Project Alpha"
4. Check attachments → None

Actions taken:
✓ Moved to: Clients/Company/Urgent
✓ Priority: Urgent (95/100)
✓ Flag: Follow up by tomorrow 5pm
✓ Notification: Sent to user
✓ Calendar: Created reminder
```

## Integration Examples

### Gmail Integration
```python
# Apply labels
def apply_gmail_labels(email_id, labels):
    service.users().messages().modify(
        userId='me',
        id=email_id,
        body={'addLabelIds': labels}
    ).execute()
```

### Outlook Integration
```python
# Move to folder
def move_to_outlook_folder(email_id, folder_path):
    outlook = win32com.client.Dispatch("Outlook.Application")
    namespace = outlook.GetNamespace("MAPI")
    folder = namespace.Folders.Item(folder_path)
    email.Move(folder)
```

## Limitations
- May need training for organization preferences
- Different email providers have different APIs
- Some categorization requires user feedback to improve
- Privacy considerations for sensitive emails
- Language barriers in non-English emails
