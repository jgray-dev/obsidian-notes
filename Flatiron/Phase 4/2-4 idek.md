
```python
class Customer(db.Model, SerializerMixin):
	__tablename__ = "customers"
	id = db.Column(db.Integer, primary_key=True)
	name = db.Column(db.String)
	email = db.Column(db.String)
	purchases = db.relationship('Purchase', back_populates='customers')
	
	serialize_rules = ('-purchases.customer')

class Purchase(db.Model, SerializerMixin):
	__tablename__ = "purchases"
	id = db.Column(db.Integer, primary_key=True)
	customer_id = db.Column(db.Integer, db.ForeignKey("customers.id"))
	product_id = db.Column(db.Integer, db.ForeignKey("products.id"))
	price = db.Column(db.Float)
	date = db.Column(db.Integer) # Unix time

#   <anything> = relationship('<CLASS NAME>', back_populates='<OWN TABLENAME>')	
	customer = relationship('Customer', back_populates='purchases')
	product = relationship('Product', back_populates='purchases')

		serialize_rules = ('-customer.purchases', '-product.purchases')`

class Product(db.Model, SerializerMixin):
	__tablename__ = "products"
	id = db.Column(db.Integer, primary_key=True)
	name = db.Column(db.String)
	company = db.Column(db.String)
	purchases = db.relationship('Purchase', back_populates='products')
	serialize_rules = ('-purchases.product')
#	serialize_rules = ('-<JOIN TABLENAME.OWN COLUMN NAME>')
```