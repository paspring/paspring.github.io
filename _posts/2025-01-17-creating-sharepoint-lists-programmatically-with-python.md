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


SharePoint is a powerful platform for collaboration and data management, and automating tasks like creating lists, renaming them, and adding columns can save a lot of time and effort. In this blog post, we'll walk through how to create SharePoint lists programmatically using Python while adhering to the **Single Responsibility Principle (SRP)** for clean and maintainable code. Additionally, we'll explore how to rename an existing list programmatically.

---

## Why Automate SharePoint List Management?

If you frequently create or update SharePoint lists with specific configurations (e.g., custom columns, list titles), automating the process ensures consistency, reduces manual effort, and eliminates errors. By writing a Python script, you can:

1. Save time with reusable code.
2. Easily configure lists using a JSON or dictionary-based structure.
3. Maintain clean and scalable code for future projects.

---

## Tools You Need

To interact with SharePoint in Python, we use the **[Office365-REST-Python-Client](https://pypi.org/project/Office365-REST-Python-Client/)** library. It enables you to perform CRUD operations on SharePoint lists, libraries, and more.

### Installation

Install the library using pip:

```bash
pip install Office365-REST-Python-Client
```

---

## The Workflow

We'll divide the workflow into these steps:

1. **Authenticate with SharePoint**: Connect to the SharePoint site using credentials.
2. **Create the SharePoint List**: Programmatically create a list with a specific template.
3. **Add Columns to the List**: Configure and add columns dynamically using a dictionary.
4. **Rename an Existing List**: Update the title of an existing SharePoint list programmatically.
5. **Combine the Steps**: Orchestrate the entire process into a clean, reusable workflow.

---

## The Code

Below is the complete implementation. Each function is designed to handle a specific task, ensuring clean and maintainable code.

### Step 1: Authenticate with SharePoint

The `authenticate_sharepoint` function connects to your SharePoint site using your tenant's **client ID** and **client secret**.

```python
from office365.runtime.auth.client_credential import ClientCredential
from office365.sharepoint.client_context import ClientContext

def authenticate_sharepoint(site_url, client_id, client_secret):
    """
    Authenticate and return the SharePoint client context.
    Args:
        site_url (str): SharePoint site URL.
        client_id (str): Client ID for authentication.
        client_secret (str): Client secret for authentication.

    Returns:
        ClientContext: Authenticated client context.
    """
    credentials = ClientCredential(client_id, client_secret)
    ctx = ClientContext(site_url).with_credentials(credentials)
    return ctx
```

---

### Step 2: Create the SharePoint List

The `create_sharepoint_list` function creates a new list using the **ListTemplateType.GenericList** template.

```python
from office365.sharepoint.lists.creation_information import ListCreationInformation
from office365.sharepoint.lists.list_template_type import ListTemplateType

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
    list_info.TemplateType = list_template
    new_list = ctx.web.lists.add(list_info)
    ctx.execute_query()
    print(f"List '{list_title}' created successfully!")
    return new_list
```

---

### Step 3: Add Columns Dynamically

The `configure_columns` function takes a configuration dictionary to define and add columns dynamically.

```python
def configure_columns(target_list, columns_config):
    """
    Add columns to the SharePoint list based on the configuration.
    Args:
        target_list (List): The SharePoint list to which columns will be added.
        columns_config (list): List of dictionaries defining column configurations.
    """
    for column in columns_config:
        column_type = column.get("type")

        if column_type == "text":
            target_list.fields.add_text(column["name"], required=column.get("required", False))
        elif column_type == "number":
            target_list.fields.add_number(column["name"], required=column.get("required", False))
        elif column_type == "choice":
            target_list.fields.add_choice(
                column["name"],
                choices=column.get("choices", []),
                required=column.get("required", False),
                default_value=column.get("default_value"),
            )
        elif column_type == "datetime":
            target_list.fields.add_datetime(
                column["name"],
                required=column.get("required", False),
                display_format=column.get("display_format", "DateOnly"),
            )
        else:
            print(f"Unsupported column type: {column_type}")
        
        # Execute after adding each column
        target_list.context.execute_query()
        print(f"Column '{column['name']}' added successfully!")
```

---

### Step 4: Rename an Existing List

The `rename_sharepoint_list` function renames an existing list by updating its `Title` property.

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
    # Get the list by its current title
    sharepoint_list = ctx.web.lists.get_by_title(current_title)
    
    # Update the Title property
    sharepoint_list.set_property("Title", new_title)
    sharepoint_list.update()
    ctx.execute_query()
    print(f"List '{current_title}' has been renamed to '{new_title}'!")
```

---

### Step 5: Combine Everything

The `create_list_with_columns` function orchestrates the workflow, combining authentication, list creation, column configuration, and renaming if necessary.

```python
def create_list_with_columns(site_url, client_id, client_secret, list_title, list_description, columns_config):
    """
    Orchestrates the creation of a SharePoint list and adding columns to it.
    Args:
        site_url (str): SharePoint site URL.
        client_id (str): Client ID for authentication.
        client_secret (str): Client secret for authentication.
        list_title (str): Title of the list.
        list_description (str): Description of the list.
        columns_config (list): List of dictionaries defining column configurations.
    """
    # Step 1: Authenticate
    ctx = authenticate_sharepoint(site_url, client_id, client_secret)

    # Step 2: Create the list
    new_list = create_sharepoint_list(ctx, list_title, list_description)

    # Step 3: Add columns to the list
    configure_columns(new_list, columns_config)

    print(f"List '{list_title}' with columns configured successfully!")
```

---

## Example Usage

### Create and Configure a List

```python
if __name__ == "__main__":
    # SharePoint site details
    site_url = "https://your-tenant.sharepoint.com/sites/your-site"
    client_id = "your-client-id"
    client_secret = "your-client-secret"

    # List details
    list_title = "My Programmatic List"
    list_description = "This list is created programmatically with Python"

    # Column configuration
    columns_config = [
        {"name": "TextColumn", "type": "text", "required": False},
        {"name": "NumberColumn", "type": "number", "required": True},
        {
            "name": "ChoiceColumn",
            "type": "choice",
            "choices": ["Option 1", "Option 2", "Option 3"],
            "required": False,
            "default_value": "Option 1",
        },
        {"name": "DateColumn", "type": "datetime", "required": False, "display_format": "DateOnly"},
    ]

    # Create the list with columns
    create_list_with_columns(site_url, client_id, client_secret, list_title, list_description, columns_config)
```

### Rename an Existing List

```python
    # Rename the existing list
    current_title = "My Programmatic List"
    new_title = "Renamed List"
    rename_sharepoint_list(ctx, current_title, new_title)
```

---

## Conclusion

Automating SharePoint list management with Python is a game-changer for developers and administrators. By leveraging the `Office365-REST-Python-Client` library and following clean coding principles like SRP, you can build flexible and maintainable scripts to handle complex workflows, including creating, configuring, and renaming lists.

Got questions or need help implementing this? Let me know in the comments below!
