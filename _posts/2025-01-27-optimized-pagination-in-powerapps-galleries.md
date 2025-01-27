---
layout: single
title: Optimized Pagination in PowerApps Galleries -A Step-by-Step Guide
date:   2025-01-21 05:31:45 +0000
author_profile: true
toc: true
toc_sticky: true
header:
  teaser: /assets/images/posts/power_apps.png
excerpt: "Pagination is an essential feature when dealing with large datasets in PowerApps. It allows users to navigate through records efficiently without overloading the app's memory."
---



# Optimized Pagination in PowerApps Galleries: A Step-by-Step Guide

Pagination is an essential feature when dealing with large datasets in PowerApps. It allows users to navigate through records efficiently without overloading the app's memory. In this blog post, we will explore how to implement optimized pagination in PowerApps galleries using delegation-friendly methods to ensure performance and scalability.

## Why Pagination Matters in PowerApps

When working with large data sources like SharePoint lists or Dataverse tables containing thousands of records, loading all data at once can significantly impact performance. PowerApps handles large datasets using delegation, which means only a subset of data is loaded at any given time. This ensures smooth app performance while minimizing memory consumption.

## Key Concepts for Pagination

To implement pagination effectively, you need to understand:

1. **Delegation:** The ability of PowerApps to push data processing to the backend data source instead of retrieving all records to the client.
2. **Hidden Gallery Technique:** Using an invisible gallery to load data in the background while optimizing queries.
3. **Pagination Variables:** Managing page size and current page number to control record display.

## Step-by-Step Implementation

### 1. Setting Up the Data Source

Connect your PowerApps gallery to your data source (e.g., SharePoint list with 5000+ records). Instead of loading all records, use delegation-friendly queries to fetch data. Set the `Items` property of a hidden gallery:

```powerapps
hiddenGallery.Items = Filter(
    StudentsList,
    Region = drpRegion.Selected.Value
)
```

In this hidden gallery, the data source is queried efficiently with filters applied.

### 2. Creating Pagination Controls

Add four buttons for pagination:
- **Next**
- **Previous**
- **First**
- **Last**

For each button, set the `OnSelect` property to update the current page number:

Set the `OnVisible` property of the screen to initialize the page number:

```powerapps
Set(varPageNumber, 1);
```

For the **Next** button, set the `OnSelect` property:

```powerapps
Set(varPageNumber, varPageNumber + 1);
```

For the **Previous** button, set the `OnSelect` property:

```powerapps
Set(varPageNumber, Max(1, varPageNumber - 1));
```

For the **First** button:

```powerapps
Set(varPageNumber, 1);
```

For the **Last** button:

```powerapps
Set(varPageNumber, RoundUp(CountRows(hiddenGallery.AllItems) / paginationSelection.Selected.Value, 0));
```

### 3. Applying Pagination Logic

Set the `Items` property of the visible gallery to manage pagination:

```powerapps
If(
    icoNext.DisplayMode = Disabled,  // Check if the 'Next' button is disabled, indicating we are on the last page
    // Handling last set of records
    LastN(
        FirstN(
            galStudentsMainHidden.AllItems,  // Get all items from the gallery
            drpPaginationSize.Selected.Value * varPageNumber  // Select items up to the current page number
        ),
        drpPaginationSize.Selected.Value - 
        (drpPaginationSize.Selected.Value * varPageNumber - Value(LblCountRows.Text))
        // Determine the number of remaining records to display when reaching the last page
    ),
    // Standard pagination handling when not on the last page
    LastN(
        FirstN(
            galStudentsMainHidden.AllItems,  // Get all items from the gallery
            drpPaginationSize.Selected.Value * varPageNumber  // Select items up to the current page number
        ),
        drpPaginationSize.Selected.Value  // Get the last set of items based on pagination size
    )
)
```

### 4. Handling the Last Page

To handle the last page correctly, set the `OnSelect` property of the **Next** button to:

```powerapps
If(
    varPageNumber * paginationSelection.Selected.Value > CountRows(hiddenGallery.AllItems),
    Set(varPageNumber, RoundUp(CountRows(hiddenGallery.AllItems) / paginationSelection.Selected.Value, 0))
)
```

### 5. Displaying Pagination Information

Set the `Text` property of a label to show pagination information:

```powerapps
"Page " & varPageNumber & " of " & RoundUp(CountRows(hiddenGallery.AllItems) / paginationSelection.Selected.Value, 0)
```

### 6. Optimized Data Loading

PowerApps loads data in batches of 100 by default. To trigger loading of the next set, set the `Default` property of the hidden gallery to the last record dynamically:

```powerapps
If(varPageNumber * paginationSelection.Selected.Value >= CountRows(hiddenGallery.AllItems),
   Last(hiddenGallery.AllItems)
)
```

## Conclusion

With this approach, you can efficiently paginate through large datasets while maintaining app performance and responsiveness. By leveraging delegation-friendly queries and using hidden galleries to optimize data retrieval, PowerApps ensures that only the necessary records are loaded at any given time.

By following these steps, you can create a seamless user experience with fast-loading galleries and intuitive navigation controls.

**Did you find this guide helpful?** Feel free to share your thoughts or ask questions in the comments below!

