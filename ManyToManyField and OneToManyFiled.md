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



OneToManyField does not exist in Django. To handle One-To-Many relationships in Django you need to use ForeignKey 1. The documentation on ForeignKey is very comprehensive and should answer all the questions you have: https://docs.djangoproject.com/en/3.2/ref/models/fields/#foreignkey 1.
For example, if you want to express that each Dude can have multiple PhoneNumbers, but the relationship should be unidirectional, you would need to add a ForeignKey field to your PhoneNumber model pointing to Dude. This would allow each number to belong to one Dude, and have Dudes able to own multiple PhoneNumbers 

what is the difference of OneToManyField and Forignkey in django?

OneToManyField does not exist in Django. Instead, to handle One-To-Many relationships in Django you need to use ForeignKey. A ForeignKey is a field that allows you to create a relationship between two models. It is used to represent a many-to-one relationship, where one instance of a model can be related to many instances of another model.

can I use related name in django?

Yes, you can use the related_name option in Django when defining a ForeignKey or a ManyToManyField. The related_name option specifies the name of the reverse relation from the related model back to the model defining the relationship. If you don’t specify a related_name, Django automatically generates one using the name of the model with the relationship and appending _set to it.
For example, if you have a ForeignKey from a model PhoneNumber to a model Dude, you can use the related_name option to specify the name of the reverse relation from Dude back to PhoneNumber. If you don’t specify a related_name, Django will automatically create a reverse relation named phonenumber_set.
