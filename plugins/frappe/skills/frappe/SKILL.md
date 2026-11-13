---
name: frappe
description: Work with data on a Frappe or ERPNext site through the frappe MCP tools. Use when the user asks to find, report on, create, change, submit or delete records (customers, invoices, items, tasks, users, any DocType) on their Frappe site, or to call a site API.
---

# Working with a Frappe site

The `frappe` MCP server connects to one Frappe site as the user who logged in. Every tool call runs with that user's roles and permissions, and runs the same validation and hooks as the desk UI. A permission error means the user cannot do it in the UI either. Do not look for a way around it.

## Find before you act

DocType and field names differ between sites, because each site has its own apps and customizations. Never guess a name.

1. `search_doctypes` to find the DocType. ERPNext uses names such as `Sales Invoice`, `Customer`, `Item`.
2. `get_doctype_schema` before you filter, create or update. It gives field names, types, mandatory fields, link targets and child tables.
3. `search_methods` or `list_document_methods` before you call a method. `get_method_schema` gives its arguments.

## Read

- `list_documents` takes `fields`, `filters`, `order_by`, `limit` and `start`, as in `GET /api/v2/document/<doctype>`.
- Filters: `{"status": "Open"}` or `[["grand_total", ">", 1000], ["posting_date", "between", ["2026-01-01", "2026-03-31"]]]`.
- Ask only for the fields you need. Use `count_documents` for totals instead of listing every row.
- `get_document` returns child tables too. Pass `fields` to keep the result small.

## Write

- Tell the user what you will create, change or delete, and get a yes before `create_document`, `update_document`, `delete_document` or a method that changes data. Do this once per batch, not once per row.
- Fill every mandatory field from the schema. Link fields take the `name` of the linked document, so look it up first.
- Child table rows go in `data` as a list under the table field name.
- Submittable DocTypes (`is_submittable`) start as Draft (`docstatus` 0). Submit or cancel them with `call_document_method` and method `submit` or `cancel`. You cannot edit a submitted document; cancel and amend it instead.
- When a call fails, read the error. It is the same message the user would see in the desk. Fix the input; do not retry the same call.

## Site instructions

The server sends the site name, the logged-in user, the installed apps and any instructions the site administrator wrote. Follow them.
