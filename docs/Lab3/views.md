# views.py

## Consignment ViewSet

```python
class ConsignmentViewSet(viewsets.ModelViewSet):
    queryset = Consignment.objects.all()
    serializer_class = serializers.ConsignmentSerialiser
    permission_classes = [IsAuthenticated]
```

- **Description**: ViewSet for the `Consignment` model, allowing CRUD operations. Requires authentication.

## Company ViewSet

```python
class CompanyViewSet(viewsets.ModelViewSet):
    queryset = Company.objects.all()
    permission_classes = [IsAuthenticated]

    def get_serializer_class(self):
        if self.action == 'count_all':
            return serializers.DateSerializer
        else:
            return serializers.CompanySerialiser

    @action(detail=False, methods=["GET"])
    def sold_most(self, request):
        # ... (method description)
```

- **Description**: ViewSet for the `Company` model, allowing CRUD operations. Provides additional actions for counting all and finding the company that sold the most.

## BrokerCompany ViewSet

```python
class BrokerCompanyViewSet(viewsets.ModelViewSet):
    queryset = BrokerCompany.objects.all()
    serializer_class = serializers.BrokerCompanySerialiser
    permission_classes = [IsAuthenticated]

    @action(detail=True, methods=["GET"])
    def never_sold(self, request, pk=None):
        # ... (method description)

    @action(detail=True, methods=["GET"])
    def salaries(self, request, pk=None):
        # ... (method description)
```

- **Description**: ViewSet for the `BrokerCompany` model, allowing CRUD operations. Provides additional actions for finding products never sold and calculating broker salaries.

## Broker ViewSet

```python
class BrokerViewSet(viewsets.ModelViewSet):
    queryset = Broker.objects.all()
    serializer_class = serializers.BrokerSerialiser
    permission_classes = [IsAuthenticated]
```

- **Description**: ViewSet for the `Broker` model, allowing CRUD operations. Requires authentication.

## Product ViewSet

```python
class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    permission_classes = [IsAuthenticated]

    def get_serializer_class(self):
        if self.action in ['count_all']:
            return serializers.DateSerializer
        else:
            return serializers.ProductSerialiser

    @action(detail=False, methods=["POST"])
    def count_all(self, request):
        # ... (method description)
```

- **Description**: ViewSet for the `Product` model, allowing CRUD operations. Provides additional actions for counting all products.

## ProductByCompany ViewSet

```python
class ProductByCompanyViewSet(viewsets.ModelViewSet):
    queryset = ProductByCompany.objects.all()
    serializer_class = serializers.ProductByCompanySerialiser
    permission_classes = [IsAuthenticated]

    @action(detail=False, methods=["GET"])
    def expired(self, request):
        # ... (method description)
```

- **Description**: ViewSet for the `ProductByCompany` model, allowing CRUD operations. Provides additional actions for finding expired products.

## BrokerWithCompany ViewSet

```python
class BrokerWithCompanyViewSet(viewsets.ModelViewSet):
    queryset = Broker.objects.all()
    serializer_class = serializers.BrokerWithCompanySerializer
    permission_classes = [IsAuthenticated]
```

- **Description**: ViewSet for the `Broker` model with additional information about the associated company. Allows CRUD operations. Requires authentication.
