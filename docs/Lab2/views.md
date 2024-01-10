# views.py

## Register User

```python
def register(request):
    if request.method == "POST":
        user_form = RegistrationForm(request.POST)
        if user_form.is_valid():
            user = user_form.save()
            user.set_password(user.password)
            user.save()
            return redirect("user_login")
    else:
        user_form = RegistrationForm()

    return render(request, "register.html", {"user_form": user_form})
```

- **Description**: Handles user registration. Validates the registration form, saves the user, and redirects to the login page upon successful registration.

## User Login

```python
def user_login(request):
    if request.method == "POST":
        user_form = LoginForm(request.POST)

        username = user_form.data.get("username")
        password = user_form.data.get("password")
        user = authenticate(username=username, password=password)

        if user is None:
            return redirect("user_login")

        login(request, user)
        return redirect("hotel_list")
    else:
        user_form = LoginForm()

    return render(request, "login.html", {"user_form": user_form})
```

- **Description**: Manages user login. Authenticates the user, logs them in, and redirects to the hotel list upon successful login.

## User Logout

```python
def user_logout(request):
    logout(request)
    return redirect("user_login")
```

- **Description**: Logs the user out and redirects to the login page.

## Hotel List

```python
def hotel_list(request):
    if request.method != "GET":
        return Http404(f"Method {request.method} not supported")

    capacity = request.GET.get("capacity", None)
    city = request.GET.get('city', None)

    available_capacities = TypeOfRoom.objects.values_list('capacity', flat=True)
    available_cities = City.objects.values_list('name', flat=True)

    hotels = Hotel.objects.all()

    if capacity is not None:
        hotels = hotels.filter(rooms__type__capacity__gte=capacity)

    if city is not None:
        hotels = hotels.filter(city__name=city)

    return render(
        request,
        "list.html",
        {
            "capacity": capacity,
            "city": city,
            "available_capacities": available_capacities,
            "available_cities": available_cities,
            "hotels": hotels
        },
    )
```

- **Description**: Retrieves and displays a list of hotels based on optional filter criteria such as capacity and city.

## Hotel Detail

```python
def hotel_detail(request, hotel_id):
    hotel = get_object_or_404(Hotel, pk=hotel_id)

    if request.method == "POST":
        comment_form = CommentForm(request.POST)
        if comment_form.is_valid():
            if Reservation.objects.filter(passenger=request.user, room__hotel=hotel_id).exists():
                comment = comment_form.save(commit=False)
                comment.hotel = Hotel.objects.get(id=hotel_id)
                comment.author = request.user
                comment.save()
        return redirect("hotel_detail", hotel_id)

    else:
        rooms_set = Room.objects.filter(hotel__id=hotel_id).order_by("number")
        rooms = []

        for room in rooms_set:
            if not (Reservation.objects.filter(room=room, date_start__lte=datetime.date.today(),
                                               date_finish__gte=datetime.date.today()).exists()):
                rooms.append(
                    {
                       "name": f"{room.number}",
                       "capacity": f"{room.type.capacity}",
                       "conveniences": f"{room.type.conveniences}",
                       "cost": f"{room.type.cost}"
                   }
                )

        comments = Comment.objects.filter(hotel=hotel)
        comment_form = CommentForm()

        return render(
           request,
           "detail.html",
           {
               "hotel": hotel,
               "comments": comments,
               "comments_exists": bool(comments.count()),
               "rooms": rooms,
               "user": request.user,
               "comment_form": comment_form
           },
       )
```

- **Description**: Displays detailed information about a specific hotel, including available rooms, comments, and handles adding comments and making reservations.

## Reserve Room

```python
@login_required(login_url='/login/')
def reserve_room(request, hotel_id):
    hotel = Hotel.objects.get(id=hotel_id)
    if request.method == "POST":
        room = Room.objects.filter(number=request.POST['room']).first()
        try:
            date_start = datetime.datetime.strptime(request.POST["date-start"], "%d.%m.%Y")
            date_finish = datetime.datetime.strptime(request.POST["date-finish"], "%d.%m.%Y")

            form = ReservationForm({"room": room, "date_start": date_start, "date_finish": date_finish})

            if not form.is_valid():
                return redirect("hotel_detail", hotel_id)

            reservation = form.save(commit=False)
            reservation.passenger = request.user
            reservation.room = room
            reservation.save()

        except Exception:
            return redirect("hotel_detail", hotel_id)

        return redirect("hotel_detail", hotel_id)

    else:
        return render(request, "reserve_room.html", {'form' : ReservationForm(), "hotel": hotel})
```

- **Description**: Allows authenticated users to reserve a room in a hotel.

## Reservations for User

```python
@login_required(login_url='/login/')
def reservations_for_user(request):
    reservations = Reservation.objects.filter(passenger=request.user)
    return render(request, "reservation_for_user.html", {"reservations": reservations})
```

- **Description**: Displays a list of reservations for the logged-in user.

## Reservation Update

```python
def reservation_update(request, reservation_id):
    reservation = get_object_or_404(Reservation, id=reservation_id)

    if request.method == "POST":
        form = ReservationForm(data=request.POST, instance=reservation)
        if not form.is_valid():
            return redirect("reservation_update", reservation_id)

        form.save()
        return redirect("hotel_detail", reservation.room.hotel.id)
    else:
        form = ReservationForm(instance=reservation)
        return render(
            request,
            "reservation_update.html",
            {"form": form, "reservation": reservation},
        )
```

- **Description**: Handles updating an existing reservation.

## Reservation Delete

```python
@login_required(login_url='/login/')
def reservation_delete(request, reservation_id):
    reservation = get_object_or_404(Reservation, id=reservation_id, passenger=request.user)

    if request.method == "POST":
        reservation.delete()
        return redirect("hotel_detail", reservation.room.hotel.id)
    else:
        return render(
            request,
            "reservation_delete.html",
            {"reservation": reservation},
        )
```

- **Description**: Handles deleting an existing reservation for the logged-in user.

## Now review the [urls.py](urls.md)