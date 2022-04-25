A post listing system that pre-renders the page, and JS picks up the subsequent querying.

To setup a post form, there are two parts, the backend and frontend.

## Backend
We'll need to inform the system of each post form, and inform it on how to react to the user's requests (e.g. from dropdowns) and what html to return too.

### Register a post form
To register a post form, add an item to the `mwb/plugin/post_forms/register` filter, here you can also apply your settings for it:
```php
/**
 * Register new post form
 * 
 * @param array $post_forms Registered post forms
 * @return array Altered registered post forms
 */
add_filter( 'mwb/plugin/post_forms/register', function( array $post_forms = [] ): array {
  $post_forms['POST-FORM-ID-HERE'] = [
    'submit_type'         => 'on-change', // on-submit / on-change (when to trigger JavaScript querying)
    'pagination_type'     => 'load-more', // load-more (NOTE: will eventually support others)
    'page_param'          => 'pg',        // change page number url parameter (useful for conflicts)
    'page_size'           => 4,           // change page size (defaults to `get_option('posts_per_page')`)
    'pagination_selector' => '.button',   // set jQuery path to pagination button (from pagination container)
    'disable_params'      => false,        // disable url params entirely (disabling SSR along with it)
    'exclude_params'      => [],           // url parameters to exclude from url (useful when current query can be derived from current page, like a term page)
  ];
  return $post_forms;
}, 10, 1 );
```

### Alter a post form query
Once registered, you'll want to determine how to query the database based on the user's selections. Use the `x` filter to adjust what is passed to `get_posts()`:

```php
/**
 * Change post form args
 * 
 * @param array $post_data $_GET or $_POST array of request
 * @param array $args Args for `get_posts()`
 * 
 * @return array Specific args for post form
 */
add_filter( 'mwb/plugin/post_forms/POST-FORM-ID-HERE/args' , function ( array $args = [], array $post_data = [] ) {

  // Set the post type(s)
  $args['post_type'] = 'post';

  // E.g. query by category slug with field with the name `cat`'s value
  if ( $cat = sanitize_text_field( $post_data['cat'] ?? false ) ) $args['category_name'] = $cat;

  return $args;

}, 1, 10 );
```

### Alter a post form's listing html (optional)
While our twig tease templates will automatically be used by post type, you do have the control to adjust the widths, breakpoints and any other resultant html.

```php
/**
 * Adjust returned html from post form results
 * 
 * @param array $html List of html for each post listing
 * @param array $post_ids List of post IDs that are being listed
 * @return array Altered list of html for each post listing
 */
add_filter( 'mwb/plugin/post_forms/POST-FORM-ID-HERE/posts_html', function( array $html = [], array $post_ids = [] ): array {

  $html = [];

  foreach ( $post_ids as $post_id ) {
    $post = \Timber::get_post( $post_id );
    $post_type = $post->post_type;
    $html[] = sprintf(
      '<li class="w-1/2 p-4 md:w-1/3 lg:w-1/4">%s</li>',
      \Timber::compile( [ "tease-$post_type.twig", 'tease-post.twig' ], [ 'post' => $post ] )
    );
  }

  return $html;

}, 10, 2 );
```

## Frontend
There is a particular setup that is required for the JavaScript to correctly pick up the inputs and know where to render the results.

Here is an example of it's most basic usage, with a content block that has passed some context about the category terms:

```twig
{% set postFormId = 'POST-FORM-ID-HERE' %}
{% set form_posts = get_init_page_form_posts( postFormId, [], false ) %}

<form
    data-post-form-id="{{ postFormId }}"
    data-post-container="#posts-{{ block.id }}"
    data-post-pagination="#posts-pagination-{{ block.id }}"
    data-post-loading="#posts-loading-{{ block.id }}"
>

    {% if filtering.categories.options %}
        <select name="{{ filtering.categories.name }}">
            <option value="">All categories</option>
            {% for term in filtering.categories.options %}
                <option value="{{ term.slug }}" {{ term.slug == filtering.categories.selected ? 'selected="selected"' }}>
                    {{ term.name }}
                </option>
            {% endfor %}
        </select>
    {% endif %}

    {{ the_fallback_update_button( 'p-2' ) }}
    {{ the_post_form_number_input( postFormId ) }}

</form>

<ul id="posts-{{ block.id }}">
    {{ form_posts.results }}
</ul>

<div id="posts-loading-{{ block.id }}" class="hidden">
    {{ mwb_icon('refresh') }}
</div>

<div id="posts-pagination-{{ block.id }}" class="{{ form_posts.more_pages == false ? 'hidden' }}">
    <a href="{{ get_page_form_next_page_link( postFormId ) }}">Load More</a>
</div>
```

FYI, below is the context that was passed to the above twig file:
```php
$data['filtering'] = [
  'categories' => [
    'selected'   => sanitize_text_field( $_GET['cat'] ?? '' ),
    'options'    => \Timber::get_terms( [ 'taxonomy' => 'category', 'hide_empty' => true ] ),
    'name'       => 'cat',
  ],
];
```

## Extra functionality
In addition to the above, there are some more features that can be used.

### "No posts found" message
Use the below filter to adjust the default "no posts found" message.

```php
/**
 * Adjust return html when post form returns no results
 */
add_filter( 'mwb/plugin/post_forms/POST-FORM-ID-HERE/empty_posts_html', function( string $html = '', array $post_data = [] ): string {
  // e.g. $html = "<li>No posts found</li>";
  return $html;
}, 10, 2 );
```

### Result counts
To give a live preview of how many results are shown out of the total, add an element to contain the message:
```twig
<div id="posts-count-{{ block.id }}">
  {{ form_posts.count_html }}
  </div>
```

This will display something like "10 posts found". If you wish for something else, use the following filter to adjust:
```php
/**
 * Adjust return html to indicate post form's result count
 */
add_filter( 'mwb/plugin/post_forms/POST-FORM-ID-HERE/count_posts_html', function( string $html = '', array $post_data = [], int $count = 0 ): string {
  // e.g. $html = sprintf( '%s post%s found', $count, $count === 1 ? '' : 's' );
  return $html;
}, 10, 3 );
```

## JavaScript events
Each event passes the post form JavaScript instance as the first parameter. This contains all of the data you'll need to react to a post form event. You'll likely want to check the post form ID if your call-back is specific to a particular instance of post form.

Example of usage:
```javascript
$(window).on('mwb/plugin/post_forms/added_posts', function(postForm) {

  // Ignore other post forms
  if (postForm.post_form_id !== 'POST-FORM-ID-HERE') return

  // Find elements with class `something` within the results, and hide them
  const $somethings = postForm.form_parts.container.find('.something')
  $somethings.hide()

})
```

### Progressing page 
- Event: `mwb/plugin/post_forms/progressing_page`
- Explanation: When a post form is about to go to the next page.
- Example usage: Update a custom current page indicator.

### Query changed 
- Event: `mwb/plugin/post_forms/query_changed`
- Explanation: When a user has changed the query (e.g. via changing a dropdown option), but the post form has yet to send a request to the server.
- Example usage: x

### Adding posts
- Event: `mwb/plugin/post_forms/adding_posts`
- Explanation: When a post form will have new results to show, but has yet to send a request to the server.
- Example usage: x.
 
### Added posts
- Event: `mwb/plugin/post_forms/added_posts`
- Explanation: When a post form has new results to show, and has received a response for it's request to the server.
- Example usage: Re-init a masonry layout.

## Common use cases

### Using a post form on a term page
When using a post form from a page that already has some indication of an existing query (such as a product category page), you'll need to alter how you setup a post form. What you'll need to do is:
1. Pass the current query's data to the form via hidden inputs (for JavaScript to later pick up on it)
1. Get the current query via `get_queried_object` (for PHP to inform from the initial page load) and via `$post_data` (for JavaScript to inform from future requests)
1. Exclude these inputs from being passed into the url params (to keep the url neat and from having redundant information)

### 1 - Pass this current query to the form
Gather the current taxonomy and term ID, and pass it through via the context.
```php
$queried_object = get_queried_object();
$filtering = [
  'taxonomy' => $queried_object->taxonomy ?? false,
  'term_id' => $queried_object->term_id ?? false,
];
return [ 'filtering' => $filtering ];
```

Within the form element, render hidden fields for the taxonomy and term ID shown below. This will tell JavaScript to pass this information back to the server.
```twig
<input type="hidden" name="taxonomy" value="{{ filtering.taxonomy }}" />
<input type="hidden" name="term_id" value="{{ filtering.term_id }}" />
```

### 2 - Get the query
Within the `mwb/plugin/post_forms/POST-FORM-ID-HERE/args` filter, get the current taxonomy and term ID, and alter the query with them. You'll have to account for the two methods this data will be passed to you. Initial server (with `get_queried_object`), and any subsequent JavaScript requests (via `$post_data`):
```php
$queried_object = get_queried_object();
$taxonomy = $queried_object->taxonomy ?? sanitize_text_field( $post_data['taxonomy'] ?? false );
$term_id = $queried_object->term_id ?? sanitize_text_field( $post_data['term_id'] ?? false );
if ( $taxonomy && $term_id ) {
  $args['tax_query'][] = [
   'taxonomy'  => $taxonomy,
   'field'     => 'term_id',
   'terms'     => $term_id,
  ];
}
```

### 3 - Exclude these inputs from url params
When registering the post form via `mwb/plugin/post_forms/register`, pass the taxonomy and term ID field names to the `exclude_params` argument, e.g.:
```php
add_filter( 'mwb/plugin/post_forms/register', function( array $post_forms = [] ): array {
  $post_forms['shop'] = [
    ...
    'exclude_params'  => [ 'taxonomy', 'term_id' ],
    ...
  ];
  return $post_forms;
}, 10, 1 );
```

This will stop taxonomy and term_id from appearing in the url as parameters. We don't need these, as the current url achieves this already for WordPress (e.g. /shop/category/baseballs/)

## Full example
See the content block OTHE/001 for a full example, with some useful default styling.
- [OTHE001.php](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/blob/master/includes/MWB/Block/OTHE/OTHE001.php)
- [OTHE001.twig](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/blob/master/resources/views/block/OTHE/OTHE001.twig)