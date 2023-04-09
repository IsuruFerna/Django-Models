# Django-Models

from django.db import models

class Author(models.Model):
    name = models.CharField(max_length=100)

class Contact(models.Model):
    author = models.OneToOneField(Author, on_delete=models.CASCADE, related_name='author_contact')
    phone = models.CharField(max_length=20)
    email = models.EmailField()
    schools = models.ManyToManyField('School', related_name='contacts')

class School(models.Model):
    name = models.CharField(max_length=100)
    
    
#now I want to acess the name of the school by knowing the name of author

author_name = 'John Doe'  # The name of the author whose schools you want to access
author = Author.objects.get(name=author_name)  # Query the Author model to get the Author object with the specified name
schools = author.author_contact.schools.all()  # Access the related Contact object and its schools attribute to get the related School objects
school_names = [school.name for school in schools]  # Get the names of the related School objects
