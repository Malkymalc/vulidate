# Form Validation in Vue.js

## DIY Form Validation

- We can use a `v-if` `<p>` to conditionally show warnings.
- We should have the form inputs as computed properties so that the warings behave reactively.
- We could v-bind `:disabled` to the submit button, with a bool (such as `!formIsValid`) as the value.
- To enhance the UX, we could for example add more precise error messages, or ensure errors are only shown when the form field has content and/or the user has clicked away.
- Vuelidate is a Vue.js plug-in that has these and other features, thus saving us 're-inventing the wheel' from scratch.

## Vuelidate
- Is model-based, decoupled from templates (and thus flexible), lightweight (and with few dependencies), and
- It comes with a wide range of validator functions, to cover most typical use cases (e.g. email, url, minLength etc.).

### Setup  
- `Vue.use(vuelidate.default);`

### Defining Rules
- This creates a new instance property:
- ```javascript
validations: {

},
```
- Inside, create a key/object pair for each property we wish to validate (mapped as per data structure of properties, as with ES6 destructuring)
- Inside the property object, we specifiy the validation rules that will apply to that property.

### Validation
- Vulidate holds all the information regarding the validation
status of our inputs in the `this.$v` object.
- We access information on the validation state of our inputs by mapping through the `$v` object to the required input, e.g. :
- ```javascript
this.$v.form.name.$invalid
```
- `$v` and all its properties are reactive, so any `$v` property referenced will update automatically (and change if required) when the form input values change.

### More detailed error messages
- Each rule for each property defined under `validations: {}` has a corresponding boolean value on the `$v` object, e.g. `$v.form.age.integer` .
- We can therefore use these booleans in e.g. `v-if` blocks in our templates:
`<p v-if="!$v.form.age.between" class="error-message">The age must be between 12 and 120</p>`
- We can ensure that the user only sees one error message at a time by using the `v-else-if` directive:
`      <p v-if="!$v.form.age.required" class="error-message">The age field is required</p>
      <p v-else-if="!$v.form.age.integer" class="error-message">The age must be an numeric whole number</p>
      <p v-else-if="!$v.form.age.between" class="error-message">The age must be between 12 and 120</p>`

### Getting Dirty with Vuelidate
- Beyond the simple boolean values, Vuelidate exposes even more detailed information that we can access:
- Besides the 'top level' `$v.form.field.$invalid` boolean, we can access the `$v.form.field.$dirty` boolean.
- This is **not** set automatically by Vuelidate, but we can set this manually, by attaching the `$v.form.field.touch()` method to an `@input` event listener on the field.


### Displaying an Error Message

## Misc

- You can add the `.number` modifier to the `v-model` directive to have user input parsed as Number type, rather than (the default) String type.
-
