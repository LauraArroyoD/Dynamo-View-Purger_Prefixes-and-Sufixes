# Revit View Purger: Automated View Cleanup

This Dynamo script provides an automated way to clean up project views that are neither placed on sheets nor tagged with specific naming conventions (prefixes or suffixes).


## User Inputs

The script features three main input nodes on the Home screen:

* **Prefix (String):** Enter the text that identifies views you want to **KEEP** (e.g., "DOC_").
* **Suffix (String):** Enter the text that identifies views you want to **KEEP** (e.g., "_STRUCTURE").
* **Run Cleanup (Boolean):** A safety switch. The operation only executes if this is set to `True`.

---

## Logic & Workflow

The core of the routine is a Python script that follows these logical steps:

### 1. Initialization
The script imports the necessary Revit API libraries (`RevitServices`, `RevitNodes`) and sets a counter variable to `0` to track the number of deleted views.

### 2. Identifying Views on Sheets
The script first collects all **Sheets** in the project. It iterates through them to find every view currently placed on a sheet, storing their IDs in a **Set**. These views are automatically exempt from deletion.

### 3. Naming Convention Filter
If a Prefix or Suffix is provided:
* The script scans all views in the project.
* It checks if a view name **Starts With** the Prefix OR **Ends With** the Suffix.
* Matching views are added to a "Protected" list.

### 4. Exclusion Logic
The algorithm calculates the final deletion list by performing a subtraction:
1.  **Total Project Views** minus **Protected (Prefix/Suffix) Views**.
2.  From that result, it removes all **Views currently on Sheets**.

### 5. Execution
If the Boolean input is `True`, a `Transaction` is started. The script deletes the remaining unprotected views and increments the counter for each