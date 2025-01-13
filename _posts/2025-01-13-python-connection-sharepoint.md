# **How to Read, Update, and Add Data to a SharePoint List Using Python**  

If you need to interact with a **SharePoint list** programmatically using Python, the `Office365-REST-Python-Client` library makes this easy by supporting SharePoint Online. Below is a guide with example code snippets for **reading**, **updating**, and **adding** data to a SharePoint list using both **username/password authentication** and **client ID/client secret (OAuth)** for secure connections.

---

## **Step 1: Install Dependencies**  

To begin, install the required library using `pip`:  

```bash
pip install Office365-REST-Python-Client
```

---

## **Step 2: Authentication Options**

You can authenticate using either **username/password** or **client ID/client secret** depending on your organization’s security setup.

### **Option 1: Username and Password**  

This option uses your SharePoint login credentials.  

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

### **Option 2: Client ID and Client Secret (OAuth)**  

This option uses **app registration credentials** (client ID and secret) to authenticate.  

```python
from office365.sharepoint.client_context import ClientContext
from office365.runtime.auth.client_credential import ClientCredential

# SharePoint site and client credentials
site_url = "https://yourcompany.sharepoint.com/sites/yoursite"
client_id = "your_client_id"
client_secret = "your_client_secret"
list_name = "Your List Name"

# Create connection
ctx = ClientContext(site_url).with_credentials(ClientCredential(client_id, client_secret))
```

---

## **Step 3: Read Items from the SharePoint List**  

This function retrieves and prints all items from the SharePoint list:  

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

This function adds a new item to the SharePoint list:  

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

This function updates the `Title` field of an item based on its ID:  

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

## **Full Python Script Example**

Below is a script that supports **read**, **add**, and **update** operations and allows you to pass the action type (`read`, `add`, or `update`) when running the script.

```python
import sys
from office365.sharepoint.client_context import ClientContext
from office365.runtime.auth.client_credential import ClientCredential
from office365.runtime.auth.user_credential import UserCredential
import os

# SharePoint site and credentials
site_url = "https://yourcompany.sharepoint.com/sites/yoursite"
list_name = "Your List Name"

# Uncomment one of the following for authentication:
# Option 1: Username and Password
username = os.getenv('SHAREPOINT_USER', "your_email@yourcompany.com")
password = os.getenv('SHAREPOINT_PASS', "your_password")
ctx = ClientContext(site_url).with_credentials(UserCredential(username, password))

# Option 2: Client ID and Client Secret
# client_id = os.getenv('CLIENT_ID', "your_client_id")
# client_secret = os.getenv('CLIENT_SECRET', "your_client_secret")
# ctx = ClientContext(site_url).with_credentials(ClientCredential(client_id, client_secret))


def read_sharepoint_list():
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    items = sharepoint_list.items.get().execute_query()
    print("SharePoint List Items:")
    for item in items:
        print(f"ID: {item.properties['Id']}, Title: {item.properties['Title']}")

def add_sharepoint_list_item():
    sharepoint_list = ctx.web.lists.get_by_title(list_name)
    new_item = {
        "Title": "New Item Title",
        "Status": "Pending"
    }
    item = sharepoint_list.add_item(new_item).execute_query()
    print(f"Added new item with ID: {item.properties['Id']}")

def update_sharepoint_list_item(item_id, new_title):
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

---

## **How to Run the Script:**

- **Read SharePoint List:**
  ```bash
  python sharepoint_script.py read
  ```
- **Add a New Item:**
  ```bash
  python sharepoint_script.py add
  ```
- **Update an Item:**
  ```bash
  python sharepoint_script.py update 1 "Updated Title"
  ```

---

## **Security Tip:**  
- Avoid hardcoding sensitive information. Use **environment variables** instead:  

**Example for setting environment variables:**
```bash
export SHAREPOINT_USER="your_email@yourcompany.com"
export SHAREPOINT_PASS="your_password"
export CLIENT_ID="your_client_id"
export CLIENT_SECRET="your_client_secret"
```

In Python:
```python
import os
username = os.getenv('SHAREPOINT_USER')
password = os.getenv('SHAREPOINT_PASS')
client_id = os.getenv('CLIENT_ID')
client_secret = os.getenv('CLIENT_SECRET')
```

---

This guide demonstrates how to efficiently interact with SharePoint lists using Python. Whether you need to read, update, or add new items, you can choose the appropriate authentication method and customize the snippets to fit your needs.

Let me know if you have any questions or need help troubleshooting!
