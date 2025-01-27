---
layout: single
title: Comprehensive Tutorial - Building an Editable Grid in PowerApps with Modern Controls
date:   2025-01-21 05:31:45 +0000
author_profile: true
toc: true
toc_sticky: true
header:
  teaser: /assets/images/posts/power_apps.png
excerpt: "Build an Excel-like grid in PowerApps! Supports CRUD, undo, sorting/filtering, and batch updates to SharePoint—no code. Modern controls for seamless data editing."
---

## **1. Introduction & Use Case**  
**Why Build an Editable Grid?**  
- **Scenario:** Imagine managing a student tracking system where teachers need to update grades, subjects, attendance, and student details in real time.  
- **Problem:** Manual data entry in SharePoint lists is time-consuming and error-prone.  
- **Solution:** A PowerApps grid that mimics Excel-like editing, with bulk updates, sorting, filtering, and undo functionality.  

**Key Features We’ll Build:**  
- 📝 **Inline Editing** (Edit any cell directly).  
- 💾 **Bulk Save** (Save all changes to SharePoint in one click).  
- ↩️ **Undo Changes** (Revert edits before saving).  
- ➕ **Add/Delete Records** (Create or remove student entries).  
- 🔍 **Dynamic Filtering** (Filter by subject or grade).  
- 🚀 **Performance** (Optimized for large datasets).  

---

## **2. Sample SharePoint Data Structure**  
**List Name:** **`Student Tracker`**  

| **Column Name**      | **Type**          | **Required?** | **Sample Data**                | **Notes**                             |  
|-----------------------|-------------------|---------------|---------------------------------|---------------------------------------|  
| **Title**             | Single line text  | Yes           | "John", "Maria"                | Renamed to "First Name" in SharePoint|  
| **Last Name**         | Single line text  | Yes           | "Doe", "Smith"                 |                                       |  
| **Subject**           | Choice            | No            | "Math", "Science", "English"   | Choices: Predefined list of subjects  |  
| **Grade**             | Lookup            | No            | "A", "B", "C"                  | Looks up to a "Grades" list           |  
| **Reported Date**     | Date              | Yes           | "2023-10-15"                   |                                       |  
| **Active**            | Yes/No            | No            | "Yes"                          | Checkbox for active/inactive status   |  

**Notes:**  
- **Grade Lookup List:** A separate SharePoint list named **`Grades`** with columns `Grade (Text)` and `Value (Number)`.  
- **Default Values:** Use SharePoint’s column settings to set defaults (e.g., `Active = Yes`).  

---

## **3. Step-by-Step Implementation**  

### **Step 1: Connect to SharePoint Data**  
1. **Create a Connection:**  
   - Go to **Data** > **Add Data** > **SharePoint**.  
   - Enter your SharePoint site URL and connect to the **`Student Tracker`** list.  
   - Verify columns match the sample table above.  

2. **Add the Grades Lookup List (Optional):**  
   - If using a lookup column for grades, connect to the **`Grades`** list.  

---

### **Step 2: Enable Modern Controls**  
1. **Activate Preview Features:**  
   - Go to **Settings** > **Upcoming Features** > **Preview**.  
   - Enable **Modern controls and themes**.  

---

### **Step 3: Build the Grid Layout**  
1. **Insert a Modern Gallery:**  
   - Go to **Insert** > **Layout** > **Blank Vertical Gallery**.  
   - Set **Items** to `Student Tracker`.  

2. **Adjust Gallery Properties:**  
   - **TemplatePadding**: `0` (Removes spacing between rows).  
   - **TemplateHeight**: `60` (Compact row height).  

3. **Add Controls to the Gallery Template:**  
   - **First Name:** Modern Text Input (`txtFirstName`, bind to `ThisItem.Title`).  
   - **Last Name:** Modern Text Input (`txtLastName`, bind to `ThisItem.LastName`).  
   - **Subject:** Modern Dropdown (`drpSubject`, bind to `Choices(StudentTracker.Subject)`).  
   - **Grade:** Modern Dropdown (`drpGrade`, bind to `Choices(StudentTracker.Grade.Value)`).  
   - **Reported Date:** Modern Date Picker (`dpReportedDate`, bind to `ThisItem.ReportedDate`).  
   - **Active:** Checkbox (`chkActive`, bind to `ThisItem.Active`).  

---

### **Step 4: Track Changes with a Collection**  
1. **Create a Hidden Tracking Button:**  
   - Add a button inside the gallery (`btnTrackChanges`).  
   - Set **Visible** to `false`.  

2. **OnSelect Logic for Tracking Edits:**  
   ```powerapps
   If(  
     IsBlank(LookUp(colGridUpdates, ID = ThisItem.ID)),  
     Collect(colGridUpdates, {  
       ID: ThisItem.ID,  
       Title: txtFirstName.Value,  
       LastName: txtLastName.Value,  
       Subject: drpSubject.Selected.Value,  
       Grade: drpGrade.Selected.Value,  
       ReportedDate: dpReportedDate.SelectedDate,  
       Active: chkActive.Checked  
     }),  
     UpdateIf(colGridUpdates,  
       ID = ThisItem.ID,  
       {  
         Title: txtFirstName.Value,  
         LastName: txtLastName.Value,  
         Subject: drpSubject.Selected.Value,  
         Grade: drpGrade.Selected.Value,  
         ReportedDate: dpReportedDate.SelectedDate,  
         Active: chkActive.Checked  
       }  
     )  
   )  
   ```  

3. **Trigger Tracking on Edit:**  
   - Set **OnChange** for all input controls to `Select(btnTrackChanges)`.  

---

### **Step 5: Save Changes to SharePoint**  
1. **Add a Save Button:**  
   - **OnSelect**:  
     ```powerapps
     Patch(  
       StudentTracker,  
       ShowColumns(colGridUpdates, "ID", "Title", "LastName", "Subject", "Grade", "ReportedDate", "Active")  
     );  
     Notify("Saved successfully!", NotificationType.Success, 3000);  
     Clear(colGridUpdates);  
     Reset(varGalleryReset) // Forces gallery to refresh  
     ```  

---

### **Step 6: Add Undo, New, and Delete Buttons**  
1. **Undo Changes:**  
   - **Button Text**: `↩️ Undo`  
   - **OnSelect**:  
     ```powerapps
     Clear(colGridUpdates);  
     Reset(varGalleryReset)  
     ```  
   - **DisplayMode**:  
     ```powerapps
     If(CountRows(colGridUpdates) = 0, Disabled, Edit)  
     ```  

2. **Create New Record:**  
   - **Button Text**: `➕ New`  
   - **OnSelect**:  
     ```powerapps
     Patch(  
       StudentTracker,  
       Defaults(StudentTracker),  
       {Title: "New", LastName: "Student", ReportedDate: Today()}  
     );  
     Reset(varGalleryReset)  
     ```  

3. **Delete Record:**  
   - Add a trash icon inside the gallery.  
   - **OnSelect**:  
     ```powerapps
     Remove(StudentTracker, ThisItem);  
     Reset(varGalleryReset)  
     ```  

---

## **4. Advanced Features**  

### **Dynamic Filtering**  
1. **Add a Filter Dropdown:**  
   - **Items**: `Choices(StudentTracker.Subject)`  
   - **OnChange**:  
     ```powerapps
     Reset(varGalleryReset)  
     ```  

2. **Update Gallery Items:**  
   ```powerapps
   Filter(  
     StudentTracker,  
     IsBlank(drpFilter.Selected.Value) || Subject.Value = drpFilter.Selected.Value  
   )  
   ```  

---

### **Sorting & Performance**  
1. **Sort by Modified Date:**  
   - Set the gallery’s **Items** to:  
     ```powerapps
     SortByColumns(StudentTracker, "Modified", Descending)  
     ```  

2. **Delegation Warning:**  
   - Use `Filter` and `Sort` with delegable operations (e.g., avoid `StartsWith` with large datasets).  

---

## **5. Testing & Validation**  
1. **Test Case 1: Edit a Cell**  
   - Change "John Doe" to "John Smith" > Click **Save** > Verify SharePoint list updates.  

2. **Test Case 2: Undo Changes**  
   - Edit multiple cells > Click **Undo** > Confirm changes revert.  

3. **Test Case 3: Add/Delete Record**  
   - Click **New** > Fill fields > Verify the record appears in SharePoint.  

---

## **6. Final Output**  
![Editable Grid Demo](https://i.imgur.com/9GkR5dH.png)  

---

## **7. Troubleshooting**  
- **Error: "Required Field Missing"**  
  - Ensure new records have values for mandatory columns (e.g., `Title`, `Last Name`).  
- **Lookup Column Not Updating**  
  - Verify the **Grades** list has valid entries and the dropdown’s `Items` property uses `Choices()`.  

---

## **8. Conclusion**  
You’ve built a **high-performance editable grid** that mirrors Excel-like functionality in PowerApps! Key takeaways:  
- Use **modern controls** for a polished UI.  
- **Collections** are critical for tracking changes before saving.  
- Always test delegation for large datasets.  

**Next Steps:**  
- Add role-based security (e.g., only teachers can edit grades).  
- Export grid data to Excel/PDF.  

**Like this tutorial?**  
👉 **Subscribe** for more PowerApps deep-dives!  
🔔 **Enable notifications** to never miss an update!  
💬 **Comment** below with your use case!
