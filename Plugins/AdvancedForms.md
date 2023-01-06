Integrates our system with [Advanced Forms](https://advancedforms.github.io/).

**This plugin is now deprecated, and you should use [GravityForms](/Plugins/GravityForms) instead**

## Functionality
- Hide forms from the block editor (to prevent submitting within the admin area)
- Adds "auto-download" functionality (see below)
- Adds a field group to the entries to inform admin of which page the submission came from (see below)
- Enables CMSable usage of placeholders (see below)
- Extends our Form field helper to include submit button styling and label choices (see below)
- Force valid email addresses for notifications (i.e. converts 'test+john@gmail.com' to 'john@gmail.com')
- Disable to view counter to prevent a database write every page load


### Auto-download
When CMSing a form within a post, you'll be provided an extra field called "Auto-download file", see below. Attaching a file here will initialise a download for the user after they submit this form.

![Auto-download file field](uploads/f134afd0fbd05fc915d3ed7d477b2ff5/image.png)

### Extra entry data
When viewing an Advanced Form entry in admin, you'll see an extra field group on the side. It'll attempt to indicate where the submission came from, see below.

![Extra entry data field group](uploads/941f87cd97ab048a2d269bb0f7e514ac/image.png)

### Select placeholders
By entering the below, and setting the select field to "required", you can use placeholders that aren't allowed to be selected.

`"" : Placeholder here`

### Form submit text and styling
When viewing an Advanced Form entry in admin, you'll be presented with "Submit text", "Button colour" and "Button styles" to customise the form's submit button, see below.

![Form selection settings](uploads/998cdc8a4ddfae8702bd9f5f20a129e1/image.png)