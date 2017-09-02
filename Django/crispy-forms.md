django-crispy-forms
===================

### Install django-crispy-forms

    $ pip install django-crispy-forms

Add the following lines to `settings.py`:

    INSTALLED_APPS = [
        'crispy_forms',
        ...
    ]

### Set your default <template_pack> for your project

Add the following line to `settings.py`:

    CRISPY_TEMPLATE_PACK = '<template_pack>' (e.g. 'bootstrap3')

### Using the crispy filter

Add the following lines to your template file:

    {% load crispy_forms_tags %}
    <form method="post" class="uniForm">
      {{ my_form|crispy }}
    </<form>

This displays a predefined form, without you having to customize anything.

### Using the {% crispy %} tag

Add the following lines to the Forms.py file inside your app folder:

    from crispy_forms.helper import FormHelper
    from crispy_forms.layout import Submit

    class ExampleForm(forms.Form):
        [...]
        def __init__(self, *args, **kwargs):
            super(ExampleForm, self).__init__(*args, **kwargs)
            self.helper = FormHelper()
            # Customize the form attributes
            #self.helper.form_id = 'id-exampleForm'
            #self.helper.form_class = 'blueForms'
            #self.helper.form_method = 'post'
            #self.helper.form_action = 'submit_form' # reverse(submit_form)
            ## Disable the inclusion of the form tag in the {% crispy %} tag
            #self.helper.form_tag = False
            ## Disable the inclusion of the csrf tag in the {% crispy %} tag
            #self.helper.disable_csrf = True
            ## If the form class is 'form-horizontal', set the size of the
            ## label and input field (for Bootstrap)
            #self.helper.label_class = 'col-md-4'
            #self.helper.field_class = 'col-md-8'
            ## Add a submit button
            #self.helper.add_input(Submit('submit', 'Submit'))
            # Define the layout for the form
            #self.helper.layout = Layout(
            #  Div(
            #      Div(HTML("""<h1> Title </h1>""")),
            #      Div('Field1', 'Field2', 'Field3', css_class='div_class'),
            #      )

Then add the following to your template file

     {% load cripsy_forms_tags %}

And the HTML form will be created according to your template pack conventions

### Modifying the layout on the go

Quick example of modifying the layout after creating it.

Change the following line from Forms.py (this is for using the filter_by_widget helper method):

    self.helper = FormHelper(self)

Add the following line after defining the layout in your constructor:

    self.helper.filter_by_widget(TextInput).wrap(Field, css_class="input-sm")

What this example does, is wrapping every field element in your layout inside a Field() class, and add the "input-sm" HTML class to them. A field is equivalent to a input HTML tag.

### Create your own helper attributes

This is as simple as passing a helper attribute to the constructor. And then editing your template_pack's target template to add a reference to your attribute, and do something with it.

For example, if I wanted to add a bootstrap info glyphicon next to my label in my forms. I should first add this to my constructor attributes:

    $ self.helper.label_info = True

And then I should edit the field.html template of the bootstrap3 template pack, to do something with my label_info atribute:

    $ cdsitepackages
    $ cd crispy_forms/templates/bootstrap3
    $ vim field.html

And add the following lines, next to your HTML label tag:

     {% if label_info %}<span class="glyphicon glyphicon-info-sign" aria-hidden="true"></span>{% endif %}

And it label_info is set to true, this will display a info glyphicon next to the label tag.



