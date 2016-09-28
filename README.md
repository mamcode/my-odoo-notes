# My Odoo notes.

* _self.env:_ Stores an execution environment.

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

* @api.returns('self', lambda rec: rec.id): This decorator maps the returned value from the new API to the old API, which is expected by the RPC protocol. In this case, the RPC calls to create expect the database id for the new record to be created, so we pass the @api.returns decorator an anonymous function, which fetches the id from the new record returned by our implementation. It is also needed if you want to extend the copy() method. Do not forget it when extending these methods if the base implementation uses the old API or you will crash with hard to interpret messages.

* UserError: Is a exception available in openerp.exceptions. A UserError will display an error message in the user interface.

* ValidationError: Is a exception available in openerp.exceptions. Is raised when a python constraint on a field is not respected.

* openerp.fields.Date.context_today(): Get the current date formatted as a string expected by create().

* openerp.fields.Date.to_string(): Converts a python datetime.date object to the expected format.

* openerp.fields.Datetime.to_string(): Converts a python datetime.datetime object to the expected format.

* openerp.fields.Date.from_string(date_str): Coverts a date formatted as a string to an datetime.date object.

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

* If for some reason you find yourself writing raw SQL queries to find record IDs, be sure to use self.env['record.model'].search([('id', 'in', tuple(ids)).ids after retrieving the IDs to make sure that security rules are applied. This is especially important in multicompany Odoo instances where record rules are used to ensure proper discrimination between companies.

* result = recordset1 + recordset2: Merge 2 recordsets into one while preserving their order.

* result = recordset1 | recordset2: Merge 2 recordsets into one ensuring that there are no duplicates in the result.

* result = recordset1 & recordset2: Finds the records that are common to two recordsets.

Summary table of the most useful python operators that can be used on recordsets:

| Operator | Action performed |
| -------- | ---------------- |
| R1 + R2  | This returns a new recordset containing the records from R1 followed by the records from R2. This can generate duplicate records in the recordset. |
| R1 – R2  | This returns a new recordset consisting of the records from R1, which are not in R2. The order is preserved. |
| R1 & R2  | This returns a new recordset with all the records that belong to both R1 and R2 (intersection of recordsets). The order is not preserved here. |
| R1 == R2 | True if both recordsets contain the same records. |
| R1 <= R2, R1 in R2 | True if all records in R1 are also in R2. Both syntaxes are equivalent. |
| R1 >= R2, R2 in R1 | True if all records in R2 are also in R1. Both syntaxes are equivalent. |
| R1 != R2 | True if R1 and R2 do not contain the same records. |

* There are also in-place operators +=, -=, &=, and |=, which modify the left-hand operand instead of creating a new recordset. These are very useful when updating a record's One2many or Many2many fields.

* The sorted() method will sort the records in a recordset. Called without arguments, the_order attribute of the model will be used. Otherwise, a function can be passed to compute a comparison key in the same fashion as the Python built-in sorted(sequence, key) function. The reverse keyword argument is also supported.

* Three options for filter records of a recordset:

```python
@api.model
def partners_with_email(self, partners):
    def predicate(partner):
        if partner.email:
           return True
        return False
    return partners.filter(predicate)
```

```python
@api.model
def partners_with_email(self, partners):
    return partners.filter(lambda p: p.email)
```

```python
partners.filter('email')
```

* recordset.mapped(path): The method mapped allows traverse the fields of the recordset. For example partner.mapped('child_ids.email') gets the email address of the contacts of the partner in a python list.

* self.user_has_groups('xml_id_of_the_group'): Check whether the user of the environment belongs to the group with XML ID "xml_id_of_the_group".

* model.fields_get(): This method is used by the web client to query for the fields of the model and their properties. It returns a Python dictionary mapping field names to a dictionary of field attributes, such as the "display" string or the "help" string.  This method can be extended (as create, write, read..) for modifying attributes of the fields of the model in the web client.

* self.with_context(key=value): Returns a new version of the recordset attached to an extended context. https://www.odoo.com/documentation/9.0/reference/orm.html#openerp.models.Model.with_context

* model.name_get(): This method is used to compute a representation of the record in various places, including in the widget used to display Many2one relations in the web client.

* model._name_search(): This method allows to customize how records are searched in the Many2one widget and other various places. The default implementation of name_search only uses the attribute referred to by the _rec_name attribute of the model class. The default implementation of name_search() actually only calls the _name_search() method, which does the real job. It is also used in the following parts of Odoo:
  - Proposals in the search widget
  - When using the in operator on One2many and Many2many fields in the domain
  - To search for records in the many2many_tags widget
  - To search for records in the CSV file import

Sources
-------

- [Book Odoo Development Cookbook.](https://www.packtpub.com/big-data-and-business-intelligence/odoo-development-cookbook)

- [Odoo Documentation](https://www.odoo.com/documentation/9.0/)
