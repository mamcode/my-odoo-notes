# My Odoo notes.

* self.env.in_onchange(): returns True if the current context of
execution is an onchange method.

* self.env.in_draft(): returns True if self is not yet committed to
the database.

* model._onchange_spec(): retrieve the onchange specifications for the model. Useful when calling onchange methods on the server side.

* model.onchange(values, ['field1'], onchange_specs): get the result of the onchange method of the model. Useful when calling onchange methods on the server side.

* dict(record._cache): get the values of the record in a dictionary. record._cache is useful when you want to get the values of the record after modifying their fields.
