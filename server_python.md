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