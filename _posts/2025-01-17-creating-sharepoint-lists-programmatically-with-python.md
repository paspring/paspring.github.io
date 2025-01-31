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

SharePoint is a powerful platform for collaboration and data management. Automating tasks like creating lists, renaming them, and adding columns can save time and effort. In this guide, you'll learn how to **create, update, and manage SharePoint lists** programmatically using Python with the `Office365-REST-Python-Client` library.  

---

## **Why Automate SharePoint List Management?**  

Manually managing SharePoint lists can be time-consuming, especially when dealing with **multiple lists or frequent updates**. By automating the process, you can:  

✅ **Save time** with reusable scripts.  
✅ **Ensure consistency** across different lists.  
✅ **Reduce human errors** when adding columns or modifying lists.  
✅ **Easily integrate with other applications** that interact with SharePoint.  

With Python, you can efficiently **create, configure, and update SharePoint lists** using an API-driven approach.  

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

We'll break the process into four steps:  

1. **Create a SharePoint List** – Programmatically create a list with a specified template.  
2. **Add Columns Dynamically** – Configure and add columns to a SharePoint list.  
3. **Update an Existing List** – Add columns programmatically to an already created SharePoint list.  
4. **Rename a List** – Modify the title of an existing SharePoint list.  

Each function will be modular, adhering to the **Single Responsibility Principle (SRP)** for clean and maintainable code.  

---

## **Step 1: Create a SharePoint List**  

The function below **creates a new SharePoint list** with a given title and description.  

```python
from office365.sharepoint.lists.creation_information import ListCreationInformation
from office365.sharepoint.lists.template_type import ListTemplateType

def create_sharepoint_list(ctx, list_title, list_description, list_template=ListTemplateType.GenericList):
    """
    Create a SharePoint list.
    Args:
        ctx (ClientContext): Authenticated SharePoint client context.
        list_title (str): Title of the list.
        list_description (str): Description of the list.
        list_template (ListTemplateType): Template type of the list.

    Returns:
        List: Created SharePoint list object.
    """
    list_info = ListCreationInformation()
    list_info.Title = list_title
    list_info.Description = list_description
    list_info.BaseTemplate = list_template  # FIXED: Use BaseTemplate instead of TemplateType

    new_list = ctx.web.lists.add(list_info)
    ctx.execute_query()

    print(f"List '{list_title}' created successfully!")
    return new_list
```

---

## **Step 2: Add Columns to a New SharePoint List**  

This function **dynamically adds columns** when creating a new SharePoint list.  

```python
from office365.sharepoint.fields.fields import Field
from office365.sharepoint.fields.field_type import FieldType

def configure_columns(target_list, columns_config):
    """
    Add columns to a SharePoint list dynamically.

    Args:
        target_list (List): The SharePoint list to which columns will be added.
        columns_config (list): List of dictionaries defining column configurations.
    """
    for column in columns_config:
        field_type = None

        if column["type"] == "text":
            field_type = FieldType.Text
        elif column["type"] == "number":
            field_type = FieldType.Number
        elif column["type"] == "choice":
            field_type = FieldType.Choice
        elif column["type"] == "datetime":
            field_type = FieldType.DateTime
        else:
            print(f"Unsupported column type: {column['type']}")
            continue

        # Create the field using XML definition
        field_xml = f'<Field Type="{field_type.value}" DisplayName="{column["name"]}" Name="{column["name"]}" />'
        field = target_list.fields.create_field_as_xml(field_xml)
        target_list.context.execute_query()

        # Configure additional properties for Choice fields
        if column["type"] == "choice":
            field.set_property("Choices", column.get("choices", []))
            field.set_property("DefaultValue", column.get("default_value", ""))
            field.update()
            target_list.context.execute_query()

        print(f"Column '{column['name']}' added successfully!")
```

---

## **Step 3: Add Columns to an Existing SharePoint List**  

Use this function to **modify an already created SharePoint list** by adding new columns dynamically.  

```python
def add_columns_to_existing_list(ctx, list_title, columns_config):
    """
    Adds columns to an existing SharePoint list based on the given configuration.

    Args:
        ctx (ClientContext): Authenticated SharePoint client context.
        list_title (str): Title of the existing SharePoint list.
        columns_config (list): List of dictionaries defining column configurations.
    
    Returns:
        None
    """
    try:
        # Retrieve the existing SharePoint list
        target_list = ctx.web.lists.get_by_title(list_title)
        ctx.load(target_list)
        ctx.execute_query()

        for column in columns_config:
            field_type = None

            if column["type"] == "text":
                field_type = FieldType.Text
            elif column["type"] == "number":
                field_type = FieldType.Number
            elif column["type"] == "choice":
                field_type = FieldType.Choice
            elif column["type"] == "datetime":
                field_type = FieldType.DateTime
            else:
                print(f"Unsupported column type: {column['type']}")
                continue

            # Create the field using XML definition
            field_xml = f'<Field Type="{field_type.value}" DisplayName="{column["name"]}" Name="{column["name"]}" />'
            field = target_list.fields.create_field_as_xml(field_xml)
            ctx.execute_query()

            # Configure additional properties for Choice fields
            if column["type"] == "choice":
                field.set_property("Choices", column.get("choices", []))
                field.set_property("DefaultValue", column.get("default_value", ""))
                field.update()
                ctx.execute_query()

            print(f"Column '{column['name']}' added successfully!")

    except Exception as e:
        print(f"Error adding columns: {e}")
```

---

## **Step 4: Rename a SharePoint List**  

```python
def rename_sharepoint_list(ctx, current_title, new_title):
    """
    Rename a SharePoint list.
    Args:
        ctx (ClientContext): Authenticated SharePoint client context.
        current_title (str): Current title of the SharePoint list.
        new_title (str): New title to update the SharePoint list.

    Returns:
        None
    """
    sharepoint_list = ctx.web.lists.get_by_title(current_title)
    sharepoint_list.set_property("Title", new_title)
    sharepoint_list.update()
    ctx.execute_query()
    print(f"List '{current_title}' has been renamed to '{new_title}'!")
```

---

## **Final Execution Workflow**  

```python
columns = [
    {"name": "ProjectName", "type": "text"},
    {"name": "Budget", "type": "number"},
    {"name": "Status", "type": "choice", "choices": ["Pending", "Approved", "Rejected"], "default_value": "Pending"},
    {"name": "StartDate", "type": "datetime"}
]

ctx = authenticate_sharepoint(site_url, client_id, client_secret)

# Create a new list and add columns
create_list_with_columns(ctx, "Project List", "A list for project tracking", columns)

# Add columns to an existing list
add_columns_to_existing_list(ctx, "Project List", columns)

# Rename the list
rename_sharepoint_list(ctx, "Project List", "Renamed Project List")
```

---

## **Conclusion**  

By automating SharePoint list management with Python, you can create, modify, and manage lists efficiently. This approach ensures **scalability**, **reusability**, and **minimal manual intervention**.  

If you have questions or suggestions, let me know in the comments! 🚀
