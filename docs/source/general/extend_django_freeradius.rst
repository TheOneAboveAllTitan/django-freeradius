======================================
Customizing django-freeradius
======================================

`django-freeeadius` provieds set of models, admin and API classes which can be imported, extende and hence customized by third party apps.

Update Settings
---------------

Update the settings to trigger the swapper:

.. code-block:: python

    #In settings.py of your project
        
    DJANGO_FREERADIUS_RADIUSREPLY_MODEL = "sample_radius.RadiusReply"
    DJANGO_FREERADIUS_RADIUSGROUPREPLY_MODEL = "sample_radius.RadiusGroupReply"
    DJANGO_FREERADIUS_RADIUSCHECK_MODEL = "sample_radius.RadiusCheck"
    DJANGO_FREERADIUS_RADIUSGROUPCHECK_MODEL = "sample_radius.RadiusGroupCheck"
    DJANGO_FREERADIUS_RADIUSACCOUNTING_MODEL = "sample_radius.RadiusAccounting"
    DJANGO_FREERADIUS_NAS_MODEL = "sample_radius.Nas"
    DJANGO_FREERADIUS_RADIUSUSERGROUP_MODEL = "sample_radius.RadiusUserGroup"
    DJANGO_FREERADIUS_RADIUSPOSTAUTHENTICATION_MODEL = "sample_radius.RadiusPostAuth"

    # where sample_radius is name of your app extending django_freeradius


Extend models
----------------
Apart from extending implemented models, `django_freeradius` also provides flexibility to extend abstract class models from `django-freeradius.base.models`.

Example:

.. code-block:: python

    #In sample_radius/models.py

    from django.db import models
    from django_freeradius.base.models import AbstractRadiusCheck
    
    class RadiusCheck(AbstractRadiusCheck):
        #modify/extend the default behavour here

        custom_field = models.TextField()


Extend admin
---------------

Similar to models, abstract admin classes from `django_freeradius.base.admin` can also be extended to avoid duplicate code.

.. code-block:: python

    # In sample_radius/admin.py
    
    from django.contrib import admin 
    from .models import RadiusCheck
    from django_freeradius.base.admin import AbstractRadiusAccountingAdmin

    class RadiusCheckAdmin(AbstractRadiusCheckAdmin):
        model = RadiusCheck

        #modify/extend default behaviour here

        def __init__(self,*args,**kwargs):
            #add your custom fields here

            self.fields.append('custom_field')
            self.list_display.append('custom_field')
            
            super(AbstractRadiusCheckAdmin, self).__init__(*args, **kwargs)

    admin.site.register(RadiusCheck,RadiusCheckAdmin)
