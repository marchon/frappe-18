## Client scripts


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