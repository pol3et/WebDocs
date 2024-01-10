# serializers.py

## Product Serializer

```python
class ProductSerialiser(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = "__all__"
```

- **Description**: Serializes the `Product` model, including all fields.

## Count Product Serializer

```python
class CountProductSerialiser(serializers.ModelSerializer):
    count = serializers.SerializerMethodField()

    class Meta:
        model = Product
        fields = ["count"]

    def get_count(self, obj):
        return ProductByCompany.objects.filter(product=obj.id, created__lt=self.context["date"]).count()
```

- **Description**: Serializes the `Product` model, including a custom count field based on related `ProductByCompany` instances filtered by date.

## Date Serializer

```python
class DateSerializer(serializers.Serializer):
    date = serializers.DateField(default=datetime.date.today())

    class Meta:
        fields = ['date']
```

- **Description**: Serializer for a date field with a default value of today.

## Product By Company Serializer

```python
class ProductByCompanySerialiser(serializers.ModelSerializer):
    class Meta:
        model = ProductByCompany
        fields = "__all__"
```

- **Description**: Serializes the `ProductByCompany` model, including all fields.

## Company Serializer

```python
class CompanySerialiser(serializers.ModelSerializer):
    class Meta:
        model = Company
        fields = "__all__"
```

- **Description**: Serializes the `Company` model, including all fields.

## Broker Serializer

```python
class BrokerSerialiser(serializers.ModelSerializer):
    class Meta:
        model = Broker
        fields = "__all__"
```

- **Description**: Serializes the `Broker` model, including all fields.

## Count Broker Salary Serializer

```python
class CountBrokerSalarySerializer(serializers.ModelSerializer):
    salary = serializers.SerializerMethodField()

    class Meta:
        model = Broker
        fields = ['id', 'name', 'salary']

    def get_salary(self, obj):
        count = 0
        for c in Consignment.objects.filter(broker=obj.id):
            count = ProductByCompany.objects.filter(consignment=c.id).count() * c.cost
        return count
```

- **Description**: Serializes the `Broker` model, including a custom salary field calculated based on related `Consignment` and `ProductByCompany` instances.

## Broker Company Serializer

```python
class BrokerCompanySerialiser(serializers.ModelSerializer):
    class Meta:
        model = BrokerCompany
        fields = "__all__"
```

- **Description**: Serializes the `BrokerCompany` model, including all fields.

## Consignment Serializer

```python
class ConsignmentSerialiser(serializers.ModelSerializer):
    class Meta:
        model = Consignment
        fields = "__all__"
```

- **Description**: Serializes the `Consignment` model, including all fields.

## Broker With Company Serializer

```python
class BrokerWithCompanySerializer(serializers.ModelSerializer):
    company_name = serializers.CharField(source='company.name', read_only=True)

    class Meta:
        model = Broker
        fields = ['id', 'name', 'income', 'company', 'company_name']
```

- **Description**: Serializes the `Broker` model, including a custom field `company_name` sourced from the related `company` field.

## [Views](views.md) are up next