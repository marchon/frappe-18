## Client scripts

* [Custom Scripts](https://github.com/frappe/erpnext/wiki/Community-Developed-Custom-Scripts)

* [Developer cheat-sheet](https://github.com/frappe/frappe/wiki/Developer-Cheatsheet)

[textbox capture focus](https://discuss.erpnext.com/t/is-there-any-tigger-to-capture-the-onclick-event-of-a-text-box/23134/3)

```
frappe.ui.form.on(cur_frm.doctype, {
    'onload_post_render': function(frm) {    
        frm.fields_dict.items.grid.wrapper.on('focus', 'input[data-fieldname="item_name"][data-doctype="Sales Invoice Item"]', function(e) {
            console.log(e.type);
        });
    }
});
```

[on change server side api call ](https://frappe.io/docs/user/en/tutorial/form-client-scripting)

```
frappe.ui.form.on("Library Transaction", "library_member",
    function(frm) {
        frappe.call({
            "method": "frappe.client.get",
            args: {
                doctype: "Library Member",
                name: frm.doc.library_member
            },
            callback: function (data) {
                frappe.model.set_value(frm.doctype,
                    frm.docname, "member_name",
                    data.message.first_name
                    + (data.message.last_name ?
                        (" " + data.message.last_name) : ""))
            }
        })
    });
```    

[link button ](https://discuss.erpnext.com/t/display-a-read-only-link-field-as-a-button/8315)
```
frappe.ui.form.on("my doctype", "my button", function(frm) { frappe.set_route("Form", ..., frm.doc.link  });
```

#### filters

```
filters: [
  ["DocType", "docfield", "op", "value"]
]
=
>
<
>=
<=
<>
like
```		

#### set value in model

```
frappe.model.set_value(doctype, name, fieldname, value)
```

```
row=$(cur_frm.fields_dict.child_table_name.grid.add_new_row())
then you can use frappe.model.set_value
e.g. you can refer this example in journal entry.js
var credit_row = frm.fields_dict.accounts.grid.add_new_row();
frappe.model.set_value(credit_row.doctype, credit_row.name, "account", values.credit_account);
```

```
frappe.ui.form.on("Grade Sheet", "score_on_form_rendered", function (doc, grid_row) {
    var grid_row = cur_frm.open_grid_row();
    grid_row.fields_dict.student.set_value("Testing");
})
```


#### [set filter in child table](https://discuss.erpnext.com/t/set-filter-in-child-table-with-value-from-same-table/10929/9)

```

frappe.ui.form.on("Sales Invoice", {
refresh : function(frm, cdt, cdn) {
	// item_code is a field in grid whose fieldname is items.. refer Sales Invoice
	frm.set_query("item_code", "items", function(doc) {
		if (frm.doc.custom_checkfield == 1) {
			return { "filters" : {"valuation_rate" : "10"}}
		} else {
			return { "filters" : {}}
		}
	});
	
	console.dir(cur_frm.fields_dict.items.grid.get_field("item_code"));
    }
);		

frappe.ui.form.on("Bank Reconciliation", "onload", function(frm) {
    cur_frm.set_query("bank_account", function() {
        return {
            "filters": {
                "account_type": "Bank",
                "group_or_ledger": "Ledger"
            }
        };
    });
});
def before_insert_customer(doc, method=None): 
    if doc.lead_name:
        for contact in frappe.get_all("Contact", {"lead": doc.lead_name}): 
            frappe.db.set_value("Contact", contact["name"], "lead", self.lead_name)
			
```


#### listview sample

```
		
			
frappe.listview_settings['Purchase Order'] = {
	add_fields: ["base_grand_total", "company", "currency", "supplier",
		"supplier_name", "per_received", "per_billed", "status"],
	get_indicator: function (doc) {
		if (doc.status === "Closed") {
			return [__("Closed"), "green", "status,=,Closed"];
		} else if (doc.status === "Delivered") {
			return [__("Delivered"), "green", "status,=,Closed"];
		} else if (flt(doc.per_received, 2) < 100 && doc.status !== "Closed") {
			if (flt(doc.per_billed, 2) < 100) {
				return [__("To Receive and Bill"), "orange",
					"per_received,<,100|per_billed,<,100|status,!=,Closed"];
			} else {
				return [__("To Receive"), "orange",
					"per_received,<,100|per_billed,=,100|status,!=,Closed"];
			}
		} else if (flt(doc.per_received, 2) == 100 && flt(doc.per_billed, 2) < 100 && doc.status !== "Closed") {
			return [__("To Bill"), "orange", "per_received,=,100|per_billed,<,100|status,!=,Closed"];
		} else if (flt(doc.per_received, 2) == 100 && flt(doc.per_billed, 2) == 100 && doc.status !== "Closed") {
			return [__("Completed"), "green", "per_received,=,100|per_billed,=,100|status,!=,Closed"];
		}
	},
	onload: function (listview) {
		var method = "erpnext.buying.doctype.purchase_order.purchase_order.close_or_unclose_purchase_orders";

		listview.page.add_menu_item(__("Close"), function () {
			listview.call_for_selected_items(method, { "status": "Closed" });
		});

		listview.page.add_menu_item(__("Re-open"), function () {
			listview.call_for_selected_items(method, { "status": "Submitted" });
		});
	}
};			
```

#### db query

```
rows = frappe.get_all("Table_name", fields = ["x", "y"])
// rows = [{"x": x_value1, "y": y_value1}, ....., {"x": x_valuen, "y": y_valuen}]

for row in frappe.get_all("Table_name", fields = ["x", "y"]):
   print row["x"]
   print row["y"]

```			

#### logging in console 

Try settings "logging": 1 in site_config.json. It will show all queries that are being executed in the javascript console.

#### redirect to report

```
frappe.set_route('query-report', 'my report name', {'filter': 'value'});
```

#### redirect to form

```
frappe.route_options = {"key": "value"}
frappe.set_route("List", "User")

```

#### dynamic filter in script report
[https://discuss.erpnext.com/t/script-report-dynamic-filter/9526](https://discuss.erpnext.com/t/script-report-dynamic-filter/9526)

```
		{
			"fieldname":"account",
			"label": __("Account"),
			"fieldtype": "Link",
			"options": "Account",
			"get_query": function() {
				var company = frappe.query_report.filters_by_name.company.get_value();
				return {
					"doctype": "Account",
					"filters": {
						"company": company,
					}
				}
			}
		},

```

#### custom button

```
frappe.ui.form.on('Gnanvidhi', {
    refresh: function(frm,cdt,cdn) {
      frm.add_custom_button(__('Show Mahatma List'), function(){
	frappe.set_route('query-report', 'Mahatma List',{gnanvidhi: locals[cdt][cdn]["name"]});
    });
  }
});
```

[https://discuss.erpnext.com/t/how-to-change-set-df-property-in-child-table-field/3545/4](https://discuss.erpnext.com/t/how-to-change-set-df-property-in-child-table-field/3545/4)

```
cur_frm.get_field("fields").grid.toggle_reqd("fieldname", true);
```


```
frappe.ui.form.on("Payment Entry", "validate", function(frm){
    var dt, references = (frm.doc.references || []).map(function(row){
         if (dt === undefined) dt = row.reference;
         return row.reference_name;
    });
    frappe.call({
        'async': false,
        'method': 'frappe.client.get_list',
        'args': {
            'doctype': dt,
           'fields': ['name', 'bill_no'],
           'filters': {'name': ['in', references]}
        },
       callback: function(res){
           (res.message || []).forEach(function(row){
               var ref = frappe.utils.filter_dict(frm.doc.references, {'reference_name': row.name})[0];
               frappe.model.set_value(ref.doctype, ref.name, "bill_no", row.bill_no);
           })
       }
    })
});
```
#### Depends on

```
eval:doc.customer && doc.customer.length
..
cur_frm.set_df_property("fieldname", "reqd", true);
```


[Filter the selections of a field in a child document](Filter the selections of a field in a child document)

```
cur_frm.fields_dict['items'].grid.get_field('item_code').get_query = function(doc, cdt, cdn) {
	return {
		filters:{'default_supplier': doc.supplier}
	}
}
```

[Sales Invoice Id Based On Sales Order Id](https://erpnext.org/docs/user/manual/en/customize-erpnext/custom-scripts/custom-script-examples/sales-invoice-id-based-on-sales-order-id)

```
frappe.ui.form.on("Sales Invoice", "refresh", function(frm){
    var sales_order = frm.doc.items[0].sales_order.replace("M", "M-");
    if (!frm.doc.__islocal && sales_order && frm.doc.name!==sales_order){
        frappe.call({
        method: 'frappe.model.rename_doc.rename_doc',
        args: {
            doctype: frm.doctype,
            old: frm.docname,
            "new": sales_order,
            "merge": false
       },
    });
    }
});
```


#### [link formatter](https://frappe.io/docs/user/en/guides/desk/formatter_for_link_fields)

```
frappe.form.link_formatters['Employee'] = function(value, doc) {
    if(doc.employee_name && doc.employee_name !== value) {
        return value + ': ' + doc.employee_name;
    } else {
        return value;
    }
}

```

#### Custom app js

1. add /public/js/file_name.js 
2. add /public/build.json file
3. change hooks.py -  app_include_js = "/assets/app_name/js/file_name.min.js"

