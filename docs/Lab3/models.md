# models.py

## Company Model

```python
class Company(models.Model):
    name = models.CharField(max_length=300)
```

- **Description**: Represents a company with a name attribute.

## Product Model

```python
class Product(models.Model):
    code = models.CharField(max_length=100, unique=True)
    name = models.CharField(max_length=300)
    unit = models.CharField(max_length=100)
    shelf_life = models.IntegerField()

    def __str__(self):
        return self.name
```

- **Description**: Represents a product with attributes such as code, name, unit, and shelf life. The `__str__` method returns the name of the product.

## BrokerCompany Model

```python
class BrokerCompany(models.Model):
    name = models.CharField(max_length=100)
    income = models.FloatField(default=0)

    def __str__(self):
        return self.name
```

- **Description**: Represents a broker company with attributes including name and income. The `__str__` method returns the name of the broker company.

## Broker Model

```python
class Broker(models.Model):
    name = models.CharField(max_length=100)
    company = models.ForeignKey(BrokerCompany, related_name='brokers', on_delete=models.CASCADE)
    income = models.FloatField(default=0)

    def __str__(self):
        return self.name
```

- **Description**: Represents a broker with attributes including name, associated company (linked to `BrokerCompany` model), and income. The `__str__` method returns the name of the broker.

## Consignment Model

```python
class Consignment(models.Model):
    broker = models.ForeignKey(Broker, related_name='what_sold', on_delete=models.CASCADE)

    date_sold = models.DateField()
    num = models.CharField(max_length=20)
    cost = models.FloatField()
    prepayment = models.BooleanField()
```

- **Description**: Represents a consignment with attributes including associated broker (linked to `Broker` model), date sold, consignment number, cost, and prepayment status.

## ProductByCompany Model

```python
class ProductByCompany(models.Model):
    company = models.ForeignKey(Company, related_name='what_produced', on_delete=models.CASCADE)
    product = models.ForeignKey(Product, related_name='produced_by', on_delete=models.CASCADE)
    consignment = models.ForeignKey(Consignment, related_name='what_in', on_delete=models.CASCADE)
    created = models.DateField()

    def __str__(self):
        return f"{self.product.name} by {self.company.name}"
```

- **Description**: Represents the relationship between a company, a product, and a consignment. The `__str__` method returns a string representation of the product produced by the company.

## [Serializers](serializers.md) next