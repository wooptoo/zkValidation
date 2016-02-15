zkValidation
============

AngularJS validation bundle (services + directives) used by [Zooku](http://my.zooku.ro).  

The validation rules are fetched from the back-end and re-used on the front-end
in order to keep the rules in sync.  
Rules allow chaining, so that a field like Email can take a validation rule like:
    
    required | email | is_available_callback

The rules follow the format described in `examples/rules.json`  
This could just as well be a dynamic end-point on your back-end.


API
===

The available directives are:

#### `validation-rule` (on form element)

Master directive fetches validation rules and sets validate-input on child inputs.

#### `validation-run-after` (on form element)

Some forms might already contain data from a backend.  
Run validation rules only after the data request was fulfilled.  

#### `validation-submit` (on submit button)

The submit button is disabled after clicking submit and waits until the request promise is fulfilled.  
The attribute value of this directive must be a Promise ($http or $resource object) in the current scope.  
The ng-disabled value must contain the formObject.$submitInProgress variable for the disable functionality to work properly.  

#### `validate` (on input field)

Validate input field.  
The `name` attribute of the field must match its rule name.

Example
=======

    <!-- Example form with validation directives -->
    <form ng-submit="editProfile(user)" name="accountForm" 
        validation-rule="rules.json"
        validation-run-after="getProfile">
        
        <label>
            First name
            <input type="text" ng-model='user.first_name' name="first_name" validate="">
        </label>
        <br>
        
        <label>
            Last name
            <input type="text" ng-model='user.last_name' name="last_name" validate="">
        </label>
        <br>
        
        <label>
            Country
            <select ng-options="c.id as c.name for c in countries" ng-model="user.country" name="country"  validate=""></select>
        </label>
        <br>
        
        <label>
            Phone number
            <input type="tel" ng-model='user.phone_number' name="phone_number" validate="">
        </label>
        <br>
        
        <label>
            Accept terms
            <input type="checkbox" ng-model="user.accept_terms" name="accept_terms" validate="">
        </label>
        <br>
        
        <button type="submit" validation-submit="saveRequest,true" 
            ng-disabled="accountForm.$invalid || accountForm.$submitInProgress"
            class="btn btn-primary">Save</button>
        
    </form>


License
=======

The project is licensed under the MIT license.
