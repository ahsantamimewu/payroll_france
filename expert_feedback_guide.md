# Guide for Expert-Comptable Feedback on BTP Payroll Rules (MVP)

This guide is intended to help the Founder facilitate a structured review session with an *expert-comptable* (chartered accountant) to validate the initial set of payroll rules designed for the BTP (construction) sector.

## 1. Objective of the Expert Review

The primary goal of this expert review is to:

*   **Validate the accuracy:** Ensure the defined payroll rules correctly reflect current French labor law and BTP-specific collective agreements (*Convention Collective Nationale du Bâtiment et des Travaux Publics - CCN BTP*).
*   **Assess completeness (for MVP):** Determine if the initial rule set adequately covers the most common and critical payroll scenarios for small BTP businesses (typically 1-20 employees) for a Minimum Viable Product.
*   **Confirm legal soundness:** Verify that the legal/conventional sources cited are appropriate and correctly interpreted.
*   **Identify potential improvements:** Gather insights on clarity, terminology, and any significant omissions or common complexities.

## 2. Materials for Review

To facilitate an effective review, please prepare and share the following with the expert-comptable:

1.  **`rulebase_btp_v1.json` File:**
    *   This JSON file contains the initial set of approximately 10-12 draft payroll rules. Each rule includes an ID, description, a condition for its application, an action (defining the calculation), and a reference to its supposed legal source.
    *   Explain that this is a programmatic representation intended to be processed by a software system.
2.  **Concrete Payroll Scenarios (3-5 examples):**
    *   In collaboration with the expert-comptable, or prepared beforehand, develop 3-5 typical payroll scenarios. These should be representative of common situations in small BTP companies.
    *   **Example Scenario:**
        *   "Employé: Karim Dubois, Catégorie: Ouvrier, Heures travaillées: 39 heures (dont 4 heures majorées à 25%), Indemnités de repas (paniers): 3, Prime de chantier: 100€, Zone de déplacement: A (3 jours concernés)."
        *   "Employée: Sophie Bernard, Catégorie: ETAM, Salaire base: 2800€ brut/mois, Heures travaillées: 35 heures, 1 jour d'absence injustifiée (8 heures), Frais de transport en commun: 75€ (abonnement mensuel)."
3.  **Manual Rule Application (Optional but Recommended):**
    *   For each scenario, attempt to manually map which rules from `rulebase_btp_v1.json` would theoretically apply and what the outcome would be. This can be a simple table or notes. This will help ground the discussion and highlight how the rules are intended to work.

## 3. Key Questions for the Expert-Comptable

Please guide the discussion using the following questions, focusing on each rule within `rulebase_btp_v1.json` and the overall set.

### Rule Accuracy:
*   For each rule, are the `condition` logic (e.g., `input.hours_worked > 35`) and the `action` (type and parameters) fields accurate for the BTP sector in France?
*   Are the specific values within `action.parameters` (e.g., overtime rates like `0.25`, fixed allowance amounts like `9.50` for meal allowance) correct and up-to-date according to the latest legal and conventional texts?
*   Are there different rates or conditions based on employee category (Ouvrier, ETAM, Cadre) within BTP that are not adequately captured for these common rules?

### Legal Soundness:
*   For each rule, are the `source.name` (e.g., 'Code du Travail', 'CCN BTP'), `source.article`, and `source.link` references appropriate and correctly cited?
*   Are there any noticeable misinterpretations of the cited legal or conventional texts in how the rule `condition` or `action` is formulated?
*   Are there more precise or authoritative articles that should be referenced instead?

### Completeness (for MVP):
*   Does this initial set of 10-12 rules cover the most critical and frequently encountered payroll elements for typical small BTP businesses (1-20 employees)?
*   Are there any glaring omissions for a Minimum Viable Product targeting this sector? Consider:
    *   Specific mandatory deductions (e.g., CSG/CRDS application, other social contributions specific to BTP like Caisse des Congés Payés - though calculation might be complex for MVP, its mention is key).
    *   Common BTP-specific premiums or indemnities not yet included (e.g., indemnité de maître d'apprentissage, specific bad weather rules - *intempéries* - if feasible for MVP).
    *   Basic handling of sickness or paid leave.

### Clarity & Terminology:
*   Is the `description` field for each rule clear, unambiguous, and easy for a non-payroll expert (like a small business owner) to understand?
*   Is the terminology used (e.g., "panier repas", "indemnité de trajet") consistent with standard BTP payroll practices and language used by *experts-comptables*?

### Edge Cases & Complexities:
*   For the rules presented, are there common edge cases or complexities in the BTP sector that the system should ideally be prepared to handle (even if noted for future development beyond MVP)?
    *   Examples:
        *   Specific conditions for meal allowances (e.g., distance from company, no canteen).
        *   Variations in travel allowances by specific *départements* or *régions* if not covered by the simplified zone model.
        *   Specific rules for apprentices, *contrats de professionnalisation*, or part-time workers in BTP.
        *   Proration rules for allowances/bonuses for employees not present the entire month.
*   How are *absences* (justified/unjustified, sickness) typically valued and deducted in BTP payslips? Does the current approach seem reasonable for an MVP?

### Scenario Validation:
*   For the 3-5 payroll scenarios presented, does the theoretical application of the existing rules from `rulebase_btp_v1.json` lead to a correct (or mostly correct for MVP) payslip calculation according to your expertise?
*   What discrepancies are observed, and which missing or incorrect rules contribute to them?

## 4. Suggested Format for Feedback

To ensure feedback is captured effectively, please discuss the preferred format with the expert-comptable. Options include:

*   **Option 1: Separate Document (Recommended for detailed comments):**
    *   A Word document, Google Doc, or Markdown file where feedback is provided per `rule.id` from `rulebase_btp_v1.json`. This allows for more extensive comments.
*   **Option 2: Direct JSON Comments (If expert is comfortable):**
    *   The expert could add a new field, e.g., `"expert_feedback": "..."`, directly into a copy of the `rulebase_btp_v1.json` file for each rule. This keeps feedback tightly coupled with the rule definition.
*   **Option 3: Spreadsheet Format:**
    *   An Excel or Google Sheets file with columns such as:
        *   `Rule ID`
        *   `Field in Question` (e.g., `condition`, `action.parameters.rate`, `source.article`)
        *   `Is this Correct? (Yes/No)`
        *   `Feedback/Correction Suggested`
        *   `Legal/Conventional Justification for Change (if any)`
        *   `Priority (High/Medium/Low for MVP)`

Regardless of the format, encourage the expert to provide:
*   **Specific examples** where rules might be misapplied or are unclear.
*   **References to legal or conventional articles** when suggesting corrections or additions.

## 5. Next Steps after Feedback

The feedback gathered from this review will be invaluable. It will be used to:

*   **Revise and refine** the initial rulebase (`rulebase_btp_v1.json`).
*   **Prioritize** the implementation of the most critical rules for the MVP.
*   **Create a new version** of the rulebase (e.g., `rulebase_btp_v2.json`) incorporating the validated changes.
*   Inform the development of the "Triple Verification Process" by understanding potential areas of legal ambiguity or common errors.

Thank you for your collaboration in making this product legally sound and truly useful for BTP businesses.
