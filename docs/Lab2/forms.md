# forms.py

## Registration Form

```python
from django import forms
from .models import Passenger, Reservation, Comment

class RegistrationForm(forms.ModelForm):
    class Meta:
        model = Passenger
        fields = ["username", "password", "first_name", "last_name", "email", "passport"]
```

- **Description**: Defines a form for user registration. Inherits from `forms.ModelForm` and includes fields for username, password, first name, last name, email, and passport.

## Login Form

```python
from django import forms
from .models import Passenger, Reservation, Comment

class LoginForm(forms.ModelForm):
    class Meta:
        model = Passenger
        fields = ["username", "password"]
```

- **Description**: Defines a form for user login. Inherits from `forms.ModelForm` and includes fields for username and password.

## Reservation Form

```python
from django import forms
from .models import Passenger, Reservation, Comment

class ReservationForm(forms.ModelForm):
    class Meta:
        model = Reservation
        fields = ["room", "date_start", "date_finish"]
```

- **Description**: Defines a form for making a reservation. Inherits from `forms.ModelForm` and includes fields for room selection, start date, and finish date.

## Comment Form

```python
from django import forms
from .models import Passenger, Reservation, Comment

class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ["rating", "text"]
```

- **Description**: Defines a form for submitting comments. Inherits from `forms.ModelForm` and includes fields for rating and text.