# models.py

## City Model

```python
class City(models.Model):
    name = models.CharField(max_length=1000)
```

- **Description**: This model represents a City with a name attribute.

## Hotel Model

```python
class Hotel(models.Model):
    name = models.CharField(max_length=50)
    owner = models.CharField(max_length=200)
    city = models.ForeignKey('hotels.City', related_name='hotels_there', on_delete=models.CASCADE)
    address = models.CharField(max_length=1000)

    def __str__(self):
        return f"{self.name}"
```

- **Description**: Represents a Hotel with attributes such as name, owner, city (linked to the City model), and address. The `__str__` method returns the name of the hotel.

## TypeOfRoom Model

```python
class TypeOfRoom(models.Model):
    name = models.CharField(max_length=20)
    capacity = models.IntegerField()
    conveniences = models.CharField(max_length=1000)
    cost = models.FloatField()
```

- **Description**: Defines a TypeOfRoom model with attributes like name, capacity, conveniences, and cost.

## Room Model

```python
class Room(models.Model):
    hotel = models.ForeignKey('hotels.Hotel', related_name='rooms', on_delete=models.CASCADE)
    type = models.ForeignKey('hotels.TypeOfRoom', related_name='rooms_of_this_type', on_delete=models.CASCADE)
    number = models.CharField(max_length=10)
```

- **Description**: Represents a Room with a hotel (linked to Hotel model), type (linked to TypeOfRoom model), and a room number.

## Reservation Model

```python
class Reservation(models.Model):
    passenger = models.ForeignKey(settings.AUTH_USER_MODEL, related_name='user_reservations', on_delete=models.CASCADE)
    room = models.ForeignKey('hotels.Room', related_name='reserved_by', on_delete=models.CASCADE)
    date_start = models.DateField()
    date_finish = models.DateField()
```

- **Description**: Defines a Reservation model, linking a passenger (user), room (linked to Room model), and reservation dates.

## Comment Model

```python
class Comment(models.Model):
    hotel = models.ForeignKey(Hotel, on_delete=models.CASCADE)
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    text = models.TextField()
    rating = models.IntegerField(validators=[MinValueValidator(1), MaxValueValidator(10)])
```

- **Description**: Represents a Comment on a Hotel, associated with an author (user), text, and a rating with validation.

## Passenger Model

```python
class Passenger(AbstractUser):
    passport = models.CharField(max_length=100)

    def __str__(self):
        return f"{self.first_name} {self.last_name} {self.passport}"
```

- **Description**: Extends the Django AbstractUser model to include a passport attribute.

## [views.py](views.md) next