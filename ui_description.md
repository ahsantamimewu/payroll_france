## UI Elements and User Flow Description

This document outlines the UI elements and user flow for the payslip generation system, based on the product brief.

### 1. WhatsApp Flow

The WhatsApp flow is designed for quick and conversational payslip generation.

**User Interaction Points & System Responses:**

*   **User Initiates:** User sends a natural language message to the designated WhatsApp number.
    *   *Example:* "Karim worked 42h with 3 meal allowances"
*   **System Confirmation:** The system processes the input and sends a confirmation message, summarizing its understanding.
    *   *UI Element:* Text message.
    *   *Example:* "OK, I've understood: Karim, 42 total hours, 3 meal allowances. Is that correct?"
*   **Clarification (If Needed):** If the input is ambiguous or incomplete, the system asks a targeted question.
    *   *UI Element:* Text message with a question.
    *   *Example:* "How many of those 42 hours were overtime?" or "For which employee are these hours?" (if not previously specified or clear from context).
*   **Legal Inconsistency/Failsafe:** If the system detects a potential legal issue (e.g., exceeding maximum work hours without proper authorization, incorrect overtime calculation based on legal minimums), it informs the user.
    *   *UI Element:* Text message explaining the inconsistency and offering expert review.
    *   *Example:* "It seems there's a potential issue: Working 42 hours with only 1 day of rest might conflict with L3132-1 (weekly rest). Would you like to flag this for expert review?"
    *   *UI Element:* Inline button/quick reply option: "Request Expert Review".
*   **User Correction/Response:** User can reply to clarify or correct information.
    *   *Example:* "Yes, that's correct" or "Actually, it was 5 hours overtime" or "Yes, please flag for review."
*   **Payslip Generation & Delivery (PDF):** Once input is confirmed (and any flags optionally acknowledged/actioned by the user), the system generates the PDF payslip.
    *   *UI Element:* WhatsApp message with the PDF document attached.
    *   *Example:* "Here is Karim's payslip." (PDF attached)
*   **Payslip Summary (Text):** Along with the PDF, the system sends a concise text summary of the payslip's key components.
    *   *UI Element:* Text message.
    *   *Example:* "Payslip ready: 42h total (35h standard + 7h overtime at +25% as per L3121-28), 3 meal allowances (CCN BTP). PDF attached."
*   **Request Expert Review (User-Initiated or Post-Generation):** The user can request an expert review at any point, or if they notice an issue with the generated payslip.
    *   *UI Element:* User can type "request expert review," "help," or a similar phrase. The system might also provide a persistent option if a potential issue was flagged earlier.
    *   *System Response:* "Understood. I've flagged this payslip for expert review. They will get in touch if needed."

### 2. Web App - Main User Flow (Input/Output)

The web app provides a richer interface for payslip generation and management.

**A. Homepage/Input Page:**

*   **Prominent Call to Action (CTA):**
    *   *UI Element:* Large, clearly visible button.
    *   *Text:* "Generate a Payslip" or "Create New Payslip".
*   **Input Method - Text:**
    *   *UI Element:* A chat-style input box, similar to messaging apps.
    *   *Placeholder Text:* "e.g., Sarah worked 35 hours regular, 5 hours overtime at 25%, and had 4 meal allowances for project Alpha" or "Type your payslip details here..."
*   **Input Method - Voice (Toggle):**
    *   *UI Element:* Microphone icon button next to or within the text input box.
    *   *Interaction:*
        *   Clicking the microphone icon starts voice input mode.
        *   Icon changes to indicate active listening (e.g., pulsing, color change).
        *   User speaks their input.
        *   Clicking again (or automatic end-of-speech detection) stops voice input.
        *   Transcribed text appears in the chat input box.

**B. Confirmation/Preview Stage:**

This stage appears after the user submits their input via text or voice.

*   **AI Understanding Summary:**
    *   *UI Element:* A structured, read-only display of the parsed information.
    *   *Content:* Clearly labeled fields and their extracted values.
        *   *Example:*
            *   Employee: Karim
            *   Total Hours: 42
            *   Regular Hours: 35
            *   Overtime Hours (+25%): 7
            *   Meal Allowances: 3
*   **Applied Rules Preview & Legal Justification:**
    *   *UI Element:* A section (collapsible accordion or list) displaying the rules the system intends to apply.
    *   *Content for each rule:*
        *   Rule Name/Description (e.g., "Overtime Pay - 25%", "Meal Allowance - CCN BTP Rate")
        *   Legal Justification (e.g., "Art. L3121-28 Code du Travail", "CCN Bâtiment et Travaux Publics - Article X.Y")
        *   Rate/Calculation Applied (e.g., "€12.50/hour", "€9.00 per allowance")
*   **Clarification Prompts (If Input Incomplete/Ambiguous):**
    *   *UI Element:* Prominently displayed questions or requests for missing information, potentially highlighted.
    *   *Example:* "Please specify the overtime rate if different from the standard 25%." or "Which employee is this payslip for?" (if not clear from input).
    *   *Interaction:* User can provide answers in a text box or select from options if applicable.

**C. Action Buttons:**

These are typically displayed below or alongside the confirmation/preview information.

*   **"Generate PDF" Button:**
    *   *UI Element:* Standard button.
    *   *Action:* Initiates the final payslip generation process based on the confirmed/clarified data.
*   **"Send via WhatsApp" Option:**
    *   *UI Element:* Button or checkbox.
    *   *Action:* If selected, after PDF generation, prompts the user for a WhatsApp number (if not already known for the employee/user) and sends the PDF and text summary via WhatsApp.
    *   *Note:* May require linking their WhatsApp account or specifying the recipient's WhatsApp-enabled phone number.
*   **"Request Expert Review" Button:**
    *   *UI Element:* Clearly labeled button, possibly with a distinct color (e.g., amber) if the system has flagged a potential issue.
    *   *Action:* Marks the payslip draft for manual review by an expert. System may ask for a brief reason for the review request.
    *   *Visibility:* Always available, but may be highlighted if the system detects inconsistencies.

**D. Output Display:**

This stage appears after the "Generate PDF" button is clicked and processing is complete.

*   **PDF Display:**
    *   *UI Element:* An embedded PDF viewer showing the generated payslip directly on the page.
    *   *Alternative/Additional UI Element:* A clear "Download PDF" link.
*   **Plain-Text Summary:**
    *   *UI Element:* A text area or formatted block displaying the key details of the payslip.
    *   *Content:* Similar to the WhatsApp text summary.
        *   *Example:* "Payslip for Karim: 42h total (35h standard + 7h overtime at +25% as per L3121-28), 3 meal allowances (CCN BTP). Total Gross: €X,XXX.XX, Total Net: €Y,YYY.YY."

**E. Navigation:**

Consistent navigation elements available throughout the web app (e.g., in a sidebar or header).

*   **"History" Link:**
    *   *UI Element:* Text link or icon button.
    *   *Action:* Takes the user to a page listing previously generated payslips with options to view, download, or potentially re-send/flag them.
*   **"Employee Management" Link:**
    *   *UI Element:* Text link or icon button.
    *   *Action:* Leads to a section for adding, viewing, or editing employee details (name, default rates, contact info, etc.).
*   **"Admin Dashboard" Link:**
    *   *UI Element:* Text link or icon button.
    *   *Visibility:* Only visible to users with administrative privileges.
    *   *Action:* Navigates to the Admin Dashboard.

### 3. Web App - Admin Dashboard

The Admin Dashboard is for oversight, managing flagged reviews, and system monitoring.

**A. Overview Section:**

*   **Key Metrics Display:**
    *   *UI Elements:* Cards, charts, or numerical displays at the top of the dashboard.
    *   *Content Examples:*
        *   "Total Payslips Generated (Last 30 Days): 150"
        *   "Payslips Flagged for Review (Current): 12"
        *   "Average Generation Time: 7 seconds"
        *   "Most Common Rule Applied: CCN BTP Meal Allowance"

**B. Flagged Reviews Section:**

*   **List/Table of Flagged Payslips:**
    *   *UI Element:* A sortable and filterable table.
    *   *Columns:*
        *   User (who generated/flagged)
        *   Employee Name
        *   Input Data Summary (e.g., "45h, 5 overtime, 2 bonuses")
        *   Reason for Flag (system-detected or user-provided)
        *   Timestamp (when flagged)
        *   Status (e.g., "Pending Review," "Reviewed," "Resolved," "Action Needed")
        *   Assigned To (if applicable, for larger teams)
*   **Actions per Flagged Entry:**
    *   *UI Element:* Buttons or icons associated with each row/entry in the table.
    *   *"View Draft" Action:* Opens a view of the payslip as it was generated, highlighting the potential issue or area of concern.
    *   *"Mark as Reviewed/Resolved" Action:* Allows admin to change the status.
        *   *Interaction:* May open a modal to add notes about the resolution or corrections made.
    *   *"Edit Payslip" Action (Optional):* If direct editing is allowed, this would take the admin to an interface to correct the payslip.
    *   *"Contact User" Action (Optional):* Shortcut to message the user who generated the payslip.

**C. Logs Section:**

*   **System Logs Access:**
    *   *UI Element:* A dedicated page or tab for viewing detailed logs.
    *   *Content:* Display of log entries, typically in reverse chronological order. Each entry might show:
        *   Timestamp
        *   User ID/Name
        *   Session ID
        *   Input Data (original user input)
        *   Parsed Data (AI's interpretation)
        *   Rules Triggered (list of rules and legal articles)
        *   GPT Prompts & Responses (if applicable and for debugging)
        *   System Actions (e.g., "PDF Generated," "Flagged for Review")
        *   Status (Success/Failure, Error messages)
*   **Filtering/Searching Capabilities:**
    *   *UI Elements:* Search bar, date range pickers, dropdown filters.
    *   *Filter Options:* By User, Date, Status (e.g., "Error," "Success," "Flagged"), Employee Name, Rule Applied.

**D. User Management (Optional - Admin Scope Dependent):**

*   **View Users:**
    *   *UI Element:* Table listing registered users.
    *   *Columns:* Username, Email, Role (e.g., "User," "Admin"), Last Active.
*   **Manage Roles:**
    *   *UI Element:* Options to assign or change user roles (if the system supports multiple roles beyond a basic user/admin split).

**E. Rulebase Management (Optional - Admin Scope Dependent):**

*   **View Current Rules:**
    *   *UI Element:* A display area (likely read-only if updates are via JSON versioning) showing the currently active rules, their conditions, calculations, and legal justifications.
    *   *Purpose:* For admins to understand how the system is currently configured and to verify rules.
*   **Interface for Update/Add Rules (If not purely JSON based):**
    *   *UI Element:* Forms or structured editors to define or modify rule parameters.
    *   *Note:* The brief mentions JSON versioning, suggesting this might be more for display/monitoring or uploading new JSON rule files rather than direct in-app editing of complex rule logic. If updates are via new JSON files, an "Upload New Rule File" option might be present.
    *   *Version History:* Display of different versions of the rulebase and when they were active.

This covers the UI elements and user flows based on the provided product brief. The focus is on clarity, efficiency, and providing necessary information and actions at each step.Okay, I have created the `ui_description.md` file with the detailed UI elements and user flow for the WhatsApp flow, the main web app user flow, and the admin dashboard.

This description includes:
- **WhatsApp Flow:** User initiation, system confirmation, clarification prompts, legal inconsistency handling, PDF payslip delivery, text summary, and the expert review option.
- **Web App Main Flow:** Homepage CTA, text/voice input, confirmation/preview stage with AI understanding and rule display, action buttons (Generate PDF, Send via WhatsApp, Request Expert Review), and output display (PDF viewer/link, text summary), along with navigation links.
- **Web App Admin Dashboard:** Overview metrics, a flagged reviews section with details and actions, a logs section with filtering/searching, and optional user/rulebase management sections.

The description aligns with the product vision of being intuitive, efficient, and transparent, especially regarding legal justifications and AI interpretations.
