# urls.py

## User Authentication URLs

```python
from django.urls import path
from . import views

urlpatterns = [
    path("register/", views.register, name="register"),
    path("login/", views.user_login, name="user_login"),
    path("logout/", views.user_logout, name="logout"),
]
```

- **Description**: Defines URL patterns for user authentication, including registration, login, and logout.

## Hotel URLs

```python
from django.urls import path
from . import views

urlpatterns += [
    path("hotels", views.hotel_list, name="hotel_list"),
    path("hotels/<int:hotel_id>", views.hotel_detail, name="hotel_detail"),
    path("hotels/<int:hotel_id>/reserve", views.reserve_room, name="reserve_room"),
]
```

- **Description**: Adds URL patterns for hotel-related views, including listing hotels, viewing hotel details, and reserving a room in a hotel.

## Reservation URLs

```python
from django.urls import path
from . import views

urlpatterns += [
    path("reservations/", views.reservations_for_user, name="reservations_for_user"),
    path("reservations/<int:reservation_id>/", views.reservation_update, name="reservation_update"),
    path("reservations/<int:reservation_id>/delete", views.reservation_delete, name="reservation_delete"),
]
```

- **Description**: Appends URL patterns for user reservations, including listing reservations, updating a reservation, and deleting a reservation.

## [forms.py](forms.md) next