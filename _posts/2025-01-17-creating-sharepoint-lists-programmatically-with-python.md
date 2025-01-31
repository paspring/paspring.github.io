---
layout: single
title: Creating SharePoint Lists Programmatically with Python - A Step-by-Step Guide
date:   2025-01-16 05:31:45 +0000
author_profile: true
toc: true
toc_sticky: true
header:
  teaser: /assets/images/posts/python_sharepoint.png
excerpt: "SharePoint is a powerful platform for collaboration and data management, and automating tasks like creating lists, renaming them, and adding columns can save a lot of time and effort"
---

SharePoint is a powerful platform for collaboration and data management. Automating tasks like **creating lists, renaming them, adding columns (individually or from a configuration file), and deleting columns** can save time and effort. In this guide, you'll learn how to **create, update, manage, and delete SharePoint lists** programmatically using Python with the `Office365-REST-Python-Client` library.  

---

## **Why Automate SharePoint List Management?**  

Manually managing SharePoint lists can be time-consuming, especially when dealing with **multiple lists or frequent updates**. By automating the process, you can:  

✅ **Save time** with reusable scripts.  
✅ **Ensure consistency** across different lists.  
✅ **Reduce human errors** when adding or modifying lists.  
✅ **Easily integrate with other applications** that interact with SharePoint.  

With Python, you can efficiently **create, configure, update, and delete SharePoint lists** using an API-driven approach.  

---

## **Tools You Need**  

We'll use the **[Office365-REST-Python-Client](https://pypi.org/project/Office365-REST-Python-Client/)** library to interact with SharePoint's REST API.  

### **Installation**  

Install the required package using:  

```bash
pip install Office365-REST-Python-Client
```

---

## **Workflow Overview**  

We'll break the process into **six** steps:  

1. **Authenticate & Connect to SharePoint**  
2. **Create a SharePoint List**  
3. **Add Columns to a SharePoint List (One by One)**  
4. **Add Multiple Columns from a Dictionary (JSON File)**  
5. **Rename a SharePoint List**  
6. **Delete a Column from an Existing SharePoint List**  

---

## **Step 1: Authenticate & Connect to SharePoint**  

```python
from office365.sharepoint.client_context import ClientContext
from office365.runtime.auth.client_credential import ClientCredential

# SharePoint site details
site_url = "https://your-tenant.sharepoint.com/sites/your-site"
client_id = "your-client-id"
client_secret = "your-client-secret"

# Authenticate and create SharePoint context
ctx = ClientContext(site_url).with_credentials(ClientCredential(client_id, client_secret))
```

✅ **Example Usage:**  
Just run this script to authenticate your SharePoint session.

---

## **Step 2: Create a SharePoint List**  

```python
from office365.sharepoint.lists.creation_information import ListCreationInformation
from office365.sharepoint.lists.template_type import ListTemplateType

def create_sharepoint_list(ctx, list_title, list_description, list_template=ListTemplateType.GenericList):
    """
    Create a SharePoint list.
    """
    list_info = ListCreationInformation()
    list_info.Title = list_title
    list_info.Description = list_description
    list_info.BaseTemplate = list_template  

    new_list = ctx.web.lists.add(list_info)
    ctx.execute_query()

    print(f"List '{list_title}' created successfully!")
    return new_list
```

✅ **Example Usage:**
```python
create_sharepoint_list(ctx, "Project Tasks", "A list to track project tasks")
```

---

## **Step 3: Add a Column to an Existing SharePoint List (One by One)**  

```python
from office365.sharepoint.fields.field_type import FieldType

def add_column_to_existing_list(ctx, list_title, column_name, column_type, choices=None, default_value=None):
    """
    Add a column to an existing SharePoint list.
    """
    try:
        target_list = ctx.web.lists.get_by_title(list_title)
        ctx.load(target_list)
        ctx.execute_query()

        field_type = None
        if column_type == "text":
            field_type = FieldType.Text
        elif column_type == "number":
            field_type = FieldType.Number
        elif column_type == "choice":
            field_type = FieldType.Choice
        elif column_type == "datetime":
            field_type = FieldType.DateTime
        else:
            print(f"Unsupported column type: {column_type}")
            return

        field_xml = f'<Field Type="{field_type.value}" DisplayName="{column_name}" Name="{column_name}" />'
        field = target_list.fields.create_field_as_xml(field_xml)
        ctx.execute_query()

        if column_type == "choice":
            field.set_property("Choices", choices or [])
            field.set_property("DefaultValue", default_value or "")
            field.update()
            ctx.execute_query()

        print(f"Column '{column_name}' added successfully to list '{list_title}'!")

    except Exception as e:
        print(f"Error adding column: {e}")
```

✅ **Examples for Adding Columns One by One:**
```python
add_column_to_existing_list(ctx, "Project Tasks", "TaskName", "text")
add_column_to_existing_list(ctx, "Project Tasks", "TaskBudget", "number")
add_column_to_existing_list(ctx, "Project Tasks", "TaskStatus", "choice", 
                            choices=["Not Started", "In Progress", "Completed"], 
                            default_value="Not Started")
add_column_to_existing_list(ctx, "Project Tasks", "DueDate", "datetime")
```

---

## **Step 4: Add Multiple Columns from a Dictionary (JSON File)**  

### **Example Configuration File (`columns_config.json`)**
```json
[
    {"name": "TaskName", "type": "text"},
    {"name": "TaskPriority", "type": "choice", "choices": ["Low", "Medium", "High"], "default_value": "Medium"},
    {"name": "TaskBudget", "type": "number"},
    {"name": "DueDate", "type": "datetime"}
]
```

### **Function to Add Columns from a JSON File**
```python
import json

def add_columns_from_config(ctx, list_title, config_file):
    """
    Add multiple columns to an existing SharePoint list using a JSON configuration file.
    """
    try:
        with open(config_file, "r") as file:
            columns_config = json.load(file)

        for column in columns_config:
            add_column_to_existing_list(ctx, list_title, column["name"], column["type"], 
                                        column.get("choices"), column.get("default_value"))

    except Exception as e:
        print(f"Error adding columns: {e}")
```

✅ **Example Usage:**
```python
add_columns_from_config(ctx, "Project Tasks", "columns_config.json")
```

---

## **Step 5: Rename a SharePoint List**  

```python
def rename_sharepoint_list(ctx, current_title, new_title):
    """
    Rename a SharePoint list.
    """
    sharepoint_list = ctx.web.lists.get_by_title(current_title)
    sharepoint_list.set_property("Title", new_title)
    sharepoint_list.update()
    ctx.execute_query()
    print(f"List '{current_title}' has been renamed to '{new_title}'!")
```

✅ **Example Usage:**
```python
rename_sharepoint_list(ctx, "Project Tasks", "Updated Project Tasks")
```

---

## **Step 6: Delete a Column from an Existing SharePoint List**  

```python
def delete_column_from_list(ctx, list_title, column_name):
    """
    Delete a column from an existing SharePoint list.
    """
    target_list = ctx.web.lists.get_by_title(list_title)
    ctx.load(target_list)
    ctx.execute_query()

    field = target_list.fields.get_by_internal_name_or_title(column_name)
    field.delete_object()
    ctx.execute_query()

    print(f"Column '{column_name}' deleted successfully from list '{list_title}'!")
```

✅ **Example Usage:**
```python
delete_column_from_list(ctx, "Project Tasks", "TaskName")
delete_column_from_list(ctx, "Project Tasks", "TaskBudget")
```

---

## **Conclusion**  

With these Python functions, you can:  
✅ **Add columns one by one**  
✅ **Use a JSON configuration file to add multiple columns**  
✅ **Easily manage SharePoint lists**  

If you have any questions, let me know in the comments! 🚀

Let me know if you have any questions! 🚀
