from django.db import models
from django.contrib.auth.models import User

class Role(models.TextChoices):
    ADMIN = 'ADMIN', 'Hospital Administrator'
    DOCTOR = 'DOCTOR', 'Doctor'
    DOCTOR_ASSISTANT = 'DOCTOR_ASSISTANT', 'Doctor Assistant'
    PATIENT = 'PATIENT', 'Patient'
    PHARMACY_STAFF = 'PHARMACY_STAFF', 'Pharmacy Staff'
    LAB_STAFF = 'LAB_STAFF', 'Laboratory Staff'

class UserProfile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE, related_name='profile')
    role = models.CharField(max_length=20, choices=Role.choices, default=Role.PATIENT)
    phone = models.CharField(max_length=15, blank=True, null=True)
    is_active = models.BooleanField(default=True)
    created_at = models.DateTimeField(auto_now_add=True)

class Patient(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE, related_name='patient_profile')
    age = models.IntegerField()
    gender = models.CharField(max_length=1, choices=[('M', 'Male'), ('F', 'Female'), ('O', 'Other')])
    phone = models.CharField(max_length=15)
    blood_group = models.CharField(max_length=10)
    address = models.TextField()

class Doctor(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE, related_name='doctor_profile')
    department = models.ForeignKey('Department', on_delete=models.SET_NULL, null=True, related_name='doctors')
    qualification = models.CharField(max_length=150)
    experience = models.CharField(max_length=50)
    phone = models.CharField(max_length=15)
    is_available = models.BooleanField(default=True)
