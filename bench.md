## bench console 


#### delete doctypes & module from bench

```

bench console
for name in frappe.get_list("Doctype", filters={"module_def": "Poll"}):
    frappe.delete_doc("Doctype", name["name"])
frappe.delete_doc("Module Def", "Poll")


```