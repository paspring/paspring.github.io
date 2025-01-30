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

This guide walks through how to build a **fully functional, editable grid** in PowerApps that integrates with **SharePoint**. The grid will support **CRUD operations** (Create, Read, Update, Delete), **undo functionality**, **sorting**, **filtering**, and **batch updates**. The approach ensures that only modified records are tracked, preventing unnecessary updates when refreshing or saving.

---

## **What You’ll Learn**
By the end of this tutorial, you’ll be able to:
1. Connect PowerApps to a **SharePoint list**.
2. Build an **editable grid** using PowerApps galleries.
3. Implement **CRUD operations** efficiently.
4. Track and manage changes using a **collection**.
5. Enable **sorting** and **filtering** without interfering with data tracking.
6. Prevent duplicate updates when refreshing or saving changes.
7. Disable **Save** and **Reset** buttons when no changes are made.

---

## **Step 1: Create a Blank Canvas App**
1. Open **PowerApps** and create a **blank canvas app**.
2. Choose the **tablet form factor** or any preferred form factor.
3. Name the app and click **Create**.

---

## **Step 2: Connect to a SharePoint List**
1. Go to the **Data Sources** pane and click **Add Data**.
2. Search for and select the **SharePoint** connector.
3. Connect to the **SharePoint site** by entering the site URL.
4. Select the **SharePoint list** to use (e.g., `StudentTracker`).
5. Click **Connect**.

---

## **Step 3: Insert a Blank Vertical Gallery**
1. Go to the **Insert** tab and select **Gallery** > **Blank Vertical Gallery**.
2. Set the **Items** property of the gallery to your SharePoint list:
   ```powerapps
   Gallery.Items = StudentTracker
   ```

---

## **Step 4: Add Editable Controls to the Gallery**
Each row in the grid should contain controls for inline editing:
- **Text Input** for text fields (`Title`, `LastName`).
- **Dropdown** for choice fields (`Subject`, `Grade`).
- **Date Picker** for date fields (`ReportedDate`).
- **Checkbox** for yes/no fields (`Active`).

Set each control’s **Default** value to match the SharePoint list column:
```powerapps
txtTitle.Default = ThisItem.Title
drpSubject.Default = ThisItem.Subject
```

---

## **Step 5: Track Changes Using a Collection**
To track modified rows **before saving them**, use a collection (`colGridUpdates`). This ensures that only changed rows are updated in SharePoint.

Modify the `OnChange` event of each control to track only real user edits:
```powerapps
If(
    ThisItem.Title <> txtTitle.Text || 
    ThisItem.Subject <> drpSubject.Selected.Value, 
    Select(hiddenEdition)
)
```

---

## **Step 6: Hidden Edition Button**
The hiddenEdition button is responsible for adding modified records to `colGridUpdaates`, ensuring that only edited rows are tracked.

```powerapps
If(
    (ThisItem.Title <> txtTitle.Text || ThisItem.Subject <> drpSubject.Selected.Value),  
    If(
        IsBlank(LookUp(colGridUpdaates, ID = ThisItem.ID)),
        Collect(colGridUpdaates, { ID: ThisItem.ID, Title: txtTitle.Text, Subject: drpSubject.Selected.Value }),
        UpdateIf(colGridUpdaates, ID = ThisItem.ID, { Title: txtTitle.Text, Subject: drpSubject.Selected.Value })
    )
)
```

---

## **Step 7: Save Button (Updating SharePoint)**
The Save button commits **only modified rows** to SharePoint.

```powerapps
Patch(
    StudentTracker,
    ShowColumns(colGridUpdaates, "Title", "Subject", "ID")
);
Clear(colGridUpdaates);
Refresh(StudentTracker);
Notify("Updated Successfully", NotificationType.Success, 3000);
```

---

## **Step 8: Undo and Reset**
An **Undo Button** allows users to discard changes before saving.

```powerapps
OnSelect:
Clear(colGridUpdaates);
Set(varReset, false);
Set(varReset, true);
```

Set its **`DisplayMode`** property to enable only when changes exist:
```powerapps
If(CountRows(colGridUpdaates) > 0, DisplayMode.Edit, DisplayMode.Disabled)
```

---

## **Step 9: Filtering**
To allow filtering **without interfering with tracking**, update the gallery’s `Items` property:

```powerapps
Gallery.Items = Filter(
    StudentTracker,
    IsBlank(drpFilter.Selected.Value) || Subject.Value = drpFilter.Selected.Value
)
```

---

## **Step 10: Testing**
Test the following:
- **Editing fields** ensures only modified rows are saved.
- **Refreshing SharePoint** does not trigger unintended updates.
- **Saving** clears `colGridUpdaates` properly.
- **Undo** cancels changes without affecting saved data.

---

## **Final Thoughts**
This guide provides a structured approach to building a **fully functional editable grid** in PowerApps using **SharePoint**. The logic ensures that only **actual changes** are tracked and prevents unnecessary updates. These optimizations improve performance, maintain data integrity, and enhance user experience.

---

Let me know if you have any questions or need further clarification.
