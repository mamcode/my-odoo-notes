# My Odoo notes.

* self.env: Stores an execution environment.

* self.env.cr: Is a database cursor.

* self.env.user: Is the user executing the action.

* self.env.context: Is the context, which is a python dictionary containing various information such as the language of the user, his configured time zone, and other specific keys that can be set at run time by the actions of the user interface.

* self.env.in_onchange(): Returns True if the current context of
execution is an onchange method.

* self.env.in_draft(): Returns True if self is not yet committed to
the database.

* model._onchange_spec(): Retrieve the onchange specifications for the model. Useful when calling onchange methods on the server side.

* model.onchange(values, ['field1'], onchange_specs): Get the result of the onchange method of the model. Useful when calling onchange methods on the server side.

* dict(record._cache): Get the values of the record in a dictionary. record._cache is useful when you want to get the values of the record after modifying their fields.

* @api.multi: Is a decorator used on methods. This decorator tells the RPC layer to initialize self using the record IDs supplied in the RPC argument ids. In such methods, self is a recordset that can refer to an arbitrary number of database records.

* @api.model: Is a decorator used on methods. Is used on methods for which only the model is important, not the contents of the recordset.

* UserError: Is a exception available in openerp.exceptions. A UserError will display an error message in the user interface.

* ValidationError: Is a exception available in openerp.exceptions. Is raised when a python constraint on a field is not respected.

* openerp.fields.Date.context_today(): Get the current date formatted as a string expected by create().

* openerp.fields.Date.to_string(): Converts a python datetime.date object to the expected format.

* openerp.fields.Datetime.to_string(): Converts a python datetime.datetime object to the expected format.

* onchange methods are not called by create(), because they are called by the web client during the initial edition of the record. Some of these methods compute default values for fields related to a given field. When creating records by hand you have to do the work yourself, either by providing explicit values or by calling the onchange methods.

* recordset.ensure_one(): Checks if the recordset contains exactly one record. This method will raise an exception if this is not the case and the processing will abort.

* partner.child_ids |= contacts: The operator |= computes the union of te current contacts of the partner and the new contacts.

* format when writing relational fields:

| Tuple | Effect |
| ------- | -------- |
| (0, 0, dict_val) | This creates a new record that will be related to the main record. |
| (1, id, dict_val) | This updates the related record with the specified ID with the supplied values. |
| (2, id) | This removes the record with the specified ID from the related records and deletes it from the database. |
| (3, id) | This removes the record with the specified ID from the related records. The record is not deleted from the database. |
| (4, id) | This adds an existing record with the supplied ID to the list of related records. |
| (5, ) | This removes all the related records, equivalent to calling (3, id) for each related id. |
| (6, 0, id_list) | This creates a relation between the record being updated and the existing record, whose IDs are in the Python list id_list. |


Sources
-------

- [Book Odoo Development Cookbook.](https://www.packtpub.com/big-data-and-business-intelligence/odoo-development-cookbook)
