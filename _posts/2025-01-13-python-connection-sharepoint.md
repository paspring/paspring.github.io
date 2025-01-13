# **How to Read, Update, and Add Data to a SharePoint List Using Python**

If you need to interact with a **SharePoint list** programmatically using Python, the `Office365-REST-Python-Client` library makes this easy by supporting SharePoint Online. Here's a step-by-step guide with example code snippets for reading, updating, and adding data to a SharePoint list.

---

## **Step 1: Install Dependencies**

To begin, install the required library using `pip`:

```bash
pip install Office365-REST-Python-Client
```

---

## **Step 2: Connect to SharePoint**

You can authenticate using your **username and password** or **client credentials**. Below is an example for basic username/password authentication:

```python
from office365.sharepoint.client_context import ClientContext
from office365.runtime.auth.user_credential import UserCredential

# SharePoint site and credentials
site_url = "https://yourcompany.sharepoint.com/sites/yoursite"
username = "your_email@yourcompany.com"
password = "your_password"
list_name = "Your List Name"

# Create connection
ctx = ClientContext(site_url).with_credentials(UserCredential(username, password))
```

---

## **Step 3: Read Items from the SharePoint List**

To read all items from a SharePoint list:

```python
def read_sharepoint_list():
    try:
        sharepoint_list = ctx.web.lists.get_by_title(list_name)
        items = sharepoint_list.items.get().execute_query()

        print("SharePoint List Items:")
        for item in items:
            print(f"ID: {item.properties['Id']}, Title: {item.properties['Title']}")
    except Exception as e:
        print(f"Error reading SharePoint list: {e}")
```

---

## **Step 4: Add an Item to the SharePoint List**

To add a new item to the SharePoint list:

```python
def add_sharepoint_list_item():
    try:
        sharepoint_list = ctx.web.lists.get_by_title(list_name)
        new_item = {
            "Title": "New Item Title",
            "Status": "Pending"
        }
        item = sharepoint_list.add_item(new_item).execute_query()
        print(f"Added new item with ID: {item.properties['Id']}")
    except Exception as e:
        print(f"Error adding item to SharePoint list: {e}")
```

---

## **Step 5: Update an Item in the SharePoint List**

To update a specific item in the SharePoint list:

```python
def update_sharepoint_list_item(item_id, new_title):
    try:
        sharepoint_list = ctx.web.lists.get_by_title(list_name)
        list_item = sharepoint_list.get_item_by_id(item_id)
        list_item.set_property('Title', new_title)
        list_item.update()
        ctx.execute_query()
        print(f"List item with ID {item_id} updated successfully.")
    except Exception as e:
        print(f"Error updating SharePoint list item: {e}")
```

---

## **Step 6: Run as a Python Script**

Here's a complete example that can be run as a `.py` file:

### **Full Code:**

```python
import sys
from office365.sharepoint.client_context import ClientContext
from office365.runtime.auth.user_credential import UserCredential

# SharePoint site and credentials
site_url = "https://yourcompany.sharepoint.com/sites/yoursite"
username = "your_email@yourcompany.com"
password = "your_password"
list_name = "Your List Name"

def read_sharepoint_list():
    ctx = ClientContext(site_url).with_credentials(UserCredential(username, password))
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    items = sharepoint_list.items.get().execute_query()
    print("SharePoint List Items:")
    for item in items:
        print(f"ID: {item.properties['Id']}, Title: {item.properties['Title']}")

def add_sharepoint_list_item():
    ctx = ClientContext(site_url).with_credentials(UserCredential(username, password))
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    new_item = {
        "Title": "New Item Title",
        "Status": "Pending"
    }
    item = sharepoint_list.add_item(new_item).execute_query()
    print(f"Added new item with ID: {item.properties['Id']}")

def update_sharepoint_list_item(item_id, new_title):
    ctx = ClientContext(site_url).with_credentials(UserCredential(username, password))
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    list_item = sharepoint_list.get_item_by_id(item_id)
    list_item.set_property('Title', new_title)
    list_item.update()
    ctx.execute_query()
    print(f"List item with ID {item_id} updated successfully.")

if __name__ == "__main__":
    action = sys.argv[1] if len(sys.argv) > 1 else "read"

    if action == "read":
        read_sharepoint_list()
    elif action == "add":
        add_sharepoint_list_item()
    elif action == "update":
        if len(sys.argv) < 4:
            print("Usage for update: python sharepoint_script.py update <item_id> <new_title>")
        else:
            item_id = int(sys.argv[2])
            new_title = sys.argv[3]
            update_sharepoint_list_item(item_id, new_title)
    else:
        print("Unknown action. Please use 'read', 'add', or 'update'.")
```

### **How to Run:**

- **Read list:**
  ```bash
  python sharepoint_script.py read
  ```
- **Add new item:**
  ```bash
  python sharepoint_script.py add
  ```
- **Update an item:**
  ```bash
  python sharepoint_script.py update 1 "Updated Title"
  ```

---

## **Security Tip:**  
Avoid hardcoding credentials. Use **environment variables**:
```bash
export SHAREPOINT_USER="your_email@yourcompany.com"
export SHAREPOINT_PASS="your_password"
```
In your Python code:
```python
import os
username = os.getenv('SHAREPOINT_USER')
password = os.getenv('SHAREPOINT_PASS')
```

---

This guide shows how to efficiently interact with SharePoint lists using Python. Whether you want to read, update, or add new items, you can customize these snippets to fit your use case.

Let me know if you have any questions or need help troubleshooting!
