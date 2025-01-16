---
layout: single
title: How to Read, Update, and Add Data to a SharePoint List Using Python
date:   2025-01-16 05:31:45 +0000
author_profile: true
toc: true
toc_sticky: true
header:
  teaser: /assets/images/posts/python_sharepoint.png
excerpt: "Here’s a complete guide to programmatically interact with **SharePoint lists** using **Python**."
---

# **How to Read, Update, and Add Data to a SharePoint List Using Python**

Here’s a complete guide to programmatically interact with **SharePoint lists** using **Python**. This post covers how to connect to SharePoint, read data, add new list items, and update existing items using **pure Python** and **Snowflake Snowpark**.

---

## **Step 1: Install Dependencies**

Before running the code, install the required libraries:
```bash
pip install Office365-REST-Python-Client snowflake-snowpark-python
```

---

## **Step 2: Authentication Options**

There are two ways to authenticate:
1. **Username/Password Authentication**  
2. **Client ID and Client Secret (OAuth with App Registration)**  

### **Option 1: Username/Password Authentication (Basic Authentication)**

This option uses your SharePoint login credentials:
```python
from office365.runtime.auth.user_credential import UserCredential

ctx = ClientContext(site_url).with_credentials(UserCredential(username, password))
```

### **Option 2: Client ID and Client Secret (OAuth Authentication)**

This option uses app registration credentials from **Azure Active Directory**:
```python
from office365.runtime.auth.client_credential import ClientCredential

ctx = ClientContext(site_url).with_credentials(ClientCredential(client_id, client_secret))
```

---

## **Step 3: Pure Python Script for SharePoint Operations**

This script can be run locally from a terminal to **read**, **add**, or **update** SharePoint list data:

```python
from office365.sharepoint.client_context import ClientContext
from office365.runtime.auth.client_credential import ClientCredential
from office365.runtime.auth.user_credential import UserCredential
import os
import sys

# SharePoint site and list details
site_url = "https://yourcompany.sharepoint.com/sites/yoursite"
list_name = "Your List Name"

# Environment variables for security
username = os.getenv('SHAREPOINT_USER', "your_email@yourcompany.com")
password = os.getenv('SHAREPOINT_PASS', "your_password")
client_id = os.getenv('CLIENT_ID', "your_client_id")
client_secret = os.getenv('CLIENT_SECRET', "your_client_secret")


def read_sharepoint_list(ctx):
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    items = sharepoint_list.items.get().execute_query()

    results = []
    for item in items:
        results.append({"ID": item.properties["Id"], "Title": item.properties["Title"]})
        print(f"ID: {item.properties['Id']}, Title: {item.properties['Title']}")
    return results


def add_sharepoint_list_item(ctx):
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    new_item = {
        "Title": "New Item Title",
        "Status": "Pending"
    }
    item = sharepoint_list.add_item(new_item).execute_query()
    print(f"Added new item with ID: {item.properties['Id']}")


def update_sharepoint_list_item(ctx, item_id, new_title):
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    list_item = sharepoint_list.get_item_by_id(item_id)
    list_item.set_property('Title', new_title)
    list_item.update()
    ctx.execute_query()
    print(f"List item with ID {item_id} updated successfully.")


def main():
    # Choose authentication type (user_credential or client_credential)
    auth_type = os.getenv("AUTH_TYPE", "user_credential").lower()

    if auth_type == "client_credential":
        ctx = ClientContext(site_url).with_credentials(ClientCredential(client_id, client_secret))
    else:
        ctx = ClientContext(site_url).with_credentials(UserCredential(username, password))

    # Perform action based on user input
    action = os.getenv("ACTION", "read").lower()

    if action == "read":
        read_sharepoint_list(ctx)
    elif action == "add":
        add_sharepoint_list_item(ctx)
    elif action == "update":
        item_id = int(os.getenv("ITEM_ID", 1))
        new_title = os.getenv("NEW_TITLE", "Updated Title")
        update_sharepoint_list_item(ctx, item_id, new_title)
    else:
        print(f"Unknown action: {action}")


if __name__ == "__main__":
    main()
```

---

## **Run the Pure Python Script**

- **Read SharePoint List:**
  ```bash
  export ACTION="read"
  python sharepoint_script.py
  ```
- **Add New Item:**
  ```bash
  export ACTION="add"
  python sharepoint_script.py
  ```
- **Update an Item:**
  ```bash
  export ACTION="update"
  export ITEM_ID=5
  export NEW_TITLE="Updated Item Title"
  python sharepoint_script.py
  ```

---

## **Step 4: Snowflake Snowpark-Compatible Python Script**

If you want to run the same script inside **Snowflake Snowpark**, update the script as follows:

```python
from office365.sharepoint.client_context import ClientContext
from office365.runtime.auth.client_credential import ClientCredential
from office365.runtime.auth.user_credential import UserCredential
from snowflake.snowpark import Session
import os

# SharePoint site and list details
site_url = "https://yourcompany.sharepoint.com/sites/yoursite"
list_name = "Your List Name"

# Environment variables for security
username = os.getenv('SHAREPOINT_USER', "your_email@yourcompany.com")
password = os.getenv('SHAREPOINT_PASS', "your_password")
client_id = os.getenv('CLIENT_ID', "your_client_id")
client_secret = os.getenv('CLIENT_SECRET', "your_client_secret")


def read_sharepoint_list(ctx):
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    items = sharepoint_list.items.get().execute_query()

    results = []
    for item in items:
        results.append({"ID": item.properties["Id"], "Title": item.properties["Title"]})
        print(f"ID: {item.properties['Id']}, Title: {item.properties['Title']}")
    return results


def add_sharepoint_list_item(ctx):
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    new_item = {
        "Title": "New Item Title",
        "Status": "Pending"
    }
    item = sharepoint_list.add_item(new_item).execute_query()
    print(f"Added new item with ID: {item.properties['Id']}")


def update_sharepoint_list_item(ctx, item_id, new_title):
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    list_item = sharepoint_list.get_item_by_id(item_id)
    list_item.set_property('Title', new_title)
    list_item.update()
    ctx.execute_query()
    print(f"List item with ID {item_id} updated successfully.")


def main(session: Session):
    auth_type = os.getenv("AUTH_TYPE", "user_credential").lower()

    if auth_type == "client_credential":
        ctx = ClientContext(site_url).with_credentials(ClientCredential(client_id, client_secret))
    else:
        ctx = ClientContext(site_url).with_credentials(UserCredential(username, password))

    action = os.getenv("ACTION", "read").lower()

    if action == "read":
        result = read_sharepoint_list(ctx)
        # Optional: Save SharePoint data in Snowflake table
        df = session.create_dataframe(result)
        df.write.mode("overwrite").save_as_table("sharepoint_list_data")
    elif action == "add":
        add_sharepoint_list_item(ctx)
    elif action == "update":
        item_id = int(os.getenv("ITEM_ID", 1))
        new_title = os.getenv("NEW_TITLE", "Updated Title")
        update_sharepoint_list_item(ctx, item_id, new_title)
    else:
        raise ValueError(f"Unknown action: {action}")

    print("Operation completed successfully.")
```

---

## **Run in Snowflake**

After deploying your Python function to Snowflake:
- **Read SharePoint Data:**
  ```sql
  CALL my_sharepoint_handler();
  ```
- **Add a New Item:**
  ```bash
  export ACTION="add"
  python sharepoint_script.py
  ```
- **Update an Item:**
  ```bash
  export ACTION="update"
  export ITEM_ID=10
  export NEW_TITLE="Updated SharePoint Item"
  python sharepoint_script.py
  ```

---

## **Security Tips:**
- **Avoid hardcoding credentials:** Use environment variables for security:
  ```bash
  export SHAREPOINT_USER="your_email@yourcompany.com"
  export SHAREPOINT_PASS="your_password"
  export CLIENT_ID="your_client_id"
  export CLIENT_SECRET="your_client_secret"
  ```

---

With this post, you now have **all the options** to interact with SharePoint lists using **Python**, whether locally or within **Snowflake Snowpark**! Let me know if you need help setting up your environment or debugging any issues!
