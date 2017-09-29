## server side python


#### DocType Events

```
autoname: Called while naming. You can set the self.name property in the method.
before_insert: Called before a document is inserted.
validate: Called before document is saved. You can throw an exception if you don't want the document to be saved
on_update: Called after the document is inserted or updated in the database.
on_submit: Called after submission.
on_cancel: Called after cancellation.
on_trash: Called after document is deleted.
```


#### api call

```
from __future__ import unicode_literals
import frappe

@frappe.whitelist()
def patient_query():#(doctype, patient_srno=None):
	data=requests.post('http://192.168.80.49:3090/api/asim/hms/history/patient', data = {"patientSrNo" : patient_srno})
	# json.loads(data.content.decode("utf-8"))
	result = data.json()
	patient =  result["result"][0] 
	print patient

	return result

bench execute healthcenter.healthcenter.doctype.patient.patient.patient_query


```

#### logging
[https://discuss.erpnext.com/t/custom-script-logging-easy-way/11984/4](https://discuss.erpnext.com/t/custom-script-logging-easy-way/11984/4)


```
log = frappe.get_logger("my_module")
log.debug("Some debug")
log.info("Some info")

//  frappe-web.log file in the frappe-bench/logs folder
 
 ```
 
 ```
 frappe.logger().debug('Custom Message')
 ```
 
#### debug sql
 
 You can always trace error in a particular query by passing debug=1 to frappe.db.sql method.
 
 
#### debugging with pycharm
[https://discuss.erpnext.com/t/how-to-debug-erpnext/514/19] (https://discuss.erpnext.com/t/how-to-debug-erpnext/514/19)



#### Custom list view.. change hooks.py

```
doctype_list_js = {
"Quotation": "path of custom_quotation_list.js",
}
```

#### Workflow status change

```
frappe.ui.form.on(DocType, "workflow_state", function(frm, cdt, cdn){
if (frm.doc.worflow_state ==== "Draft"){
// do something
}else{
// do something
}
});
```


[upload-attachment-using-rest-api](https://discuss.erpnext.com/t/upload-attachment-using-rest-api/4770)
