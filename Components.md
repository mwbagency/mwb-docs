Components are features that aren't an MWB Plugin, but are significant within the system:
- [Buttons](#buttons) // TODO
- [Carousels](#carousels)

## Carousels 
Using [slick](https://kenwheeler.github.io/slick/) we can initialise a carousel just like before. However, in twig there is a difference of syntax, i.e.:
```
{%
set config = {
	arrows: false,
	adaptiveHeight: false,
	dots: false,
	infinite: true,
	slidesToShow: 1,
	slidesToScroll: 1,
	mobileFirst: true,
	responsive: [
		{
			breakpoint: 640,
			settings: {
				slidesToShow: 2,
				slidesToScroll: 2,
			}
		},
	],
}
%}

<ul data-slick='{{ config|json_encode|e('html') }}'>
	{% for item in items %}
		{# slide html here #}
	{% endfor %}
</ul>
```

There are also some enhancements built in for carousels.


**Helper classes** - to get more context of the carousel's config from the context of CSS, we automatically apply the following classes to the carousel element (i.e. `.slick-slider`):
* `slick-arrowed` if arrows are enabled
* `slick-adaptive-height` if adaptive height is enabled

**Same height** - carousels that are horizontal, have adaptive height and only 1 row automatically have their slides set to the same height.

**Arrow html** - some default modules use custom html for the arrow html. This html is available from the [timber](https://timber.github.io/docs/) context, and can be adjusted via the filters `mwb/site/carousel_arrow_html_prev` and `mwb/site/carousel_arrow_html_next` (see `functions.example.php`). These arrows simply have the `left` and `right` icons injected into their html. To use these in a carousel config, see example:
```
{%
set config = {
    ...
    prevArrow: carousel_arrow_html.prev,
    nextArrow: carousel_arrow_html.next,
    ....
}
%}
```

**Conditional classes** - as we are using Tailwind, being able to set classes based on state can be very powerful.
* Slide elements with the data attribute `data-slick-active="CLASS-HERE"` will get this class toggled when active / inactive (FYI - 'active' means the slide is visible).
* Slide elements with the data attribute `data-slick-current="CLASS-HERE"` will get this class toggled when current / not current (FYI - only one slide is 'current' at a time).

**Styling** - by default the system doesn't apply any custom styling, this gives you a standard "base" to work from. However, there are some classes you can apply that you may find useful. If you follow the below structure of classes and elements, you get a carousel which follows the sizing and padding of the `container` class, whilst also not clipping anything horizontally when changing slides. It also attempts to style the arrows and dots inoffensively (if present).
```
<div class="carousel-container">
	<div class="carousel">
		<div class="fade-inactive-slides" data-slick='{{ config|json_encode|e('html') }}'>
			{# ... #}
		</div>
	</div>
</div>
```
_Note: there is also the optional class `fade-inactive-slides` shown above which when applied to the carousel element will fade out inactive slides for a smoother transition._

**External / custom elements** - if you don't want to use the carousel's default injected elements/html, you can declare your own, this is particularly useful for more complex layouts, as you can put your arrows, dots and counters (e.g. "5 out of 10 slides") whether you want.

Currently only `arrows`, `counters` and `dots` are supported.

Arrows - declare the CSS selectors for the prev and next buttons on the carousel element (i.e. `.slick-slider`) using the data attributes `data-slick-arrow-prev` and `data-slick-arrow-next`. On the external arrow elements, you also have access to setting a class when they're disabled using the `data-slick-arrow-disabled` data attribute. E.g.:
```
<ul
	data-slick='{{ config|json_encode|e('html') }}'
	data-slick-arrow-prev=".arrow-prev-{{ block.id }}"
	data-slick-arrow-next=".arrow-next-{{ block.id }}"
>
	{# ... #}
</ul>


{# anywhere else on the page, include the below #}

<a
	href="javascript:;"
	class="arrow-prev-{{ block.id }} text-link"
	data-slick-arrow-disabled="opacity-50 pointer-events-none"
>
	Prev
</a>

<a
	href="javascript:;"
	class="arrow-next-{{ block.id }} text-link"
	data-slick-arrow-disabled="opacity-50 pointer-events-none"
>
	Next
</a>
```

_Note: in the below example, you'll also see the use of `block.id`, this is useful only when using these external elements in a block. This is so that multiple sliders on the same page from the same block type won't interact with each other._

Counters - declare the CSS selectors for any counter elements on the carousel element (i.e. `.slick-slider`) using the data attribute `data-slick-counters`. Within any counter element, you can declare which child element should show the "current" slide, and "total" slide counts. E.g.:
```
<ul
	data-slick='{{ config|json_encode|e('html') }}'
	data-slick-counters=".counter-{{ block.id }}"
>
	{# ... #}
</ul>


{# anywhere else on the page, include the below #}

<div class="counter-{{ block.id }}">
	<span data-slick-counter-current></span> / <span data-slick-counter-total></span>
</div>
```

Dots - declare the CSS selectors for any dot container elements on the carousel element (i.e. `.slick-slider`) using the data attribute `data-slick-dots`. Within any dot container element, you can declare the classes to be applied to the dots inside the container. 
* `data-slick-dot` - generic classes for all dots regardless of their state
* `data-slick-dot-inactive` - classes for all inactive dots (removed when the dot is active)
* `data-slick-dot-active` - classes for active dots (removed when the dot is inactive)

E.g.:
```
<ul
	data-slick='{{ config|json_encode|e('html') }}'
	data-slick-dots=".dot-{{ block.id }}"
>
	{# ... #}
</ul>


{# anywhere else on the page, include the below #}

<ul 
	class="dot-{{ block.id }} flex flex-row items-center justify-center p-4" 
	data-slick-dot="m-2 w-3 h-3 text-link border border-primary rounded-full focus:outline-none transition-all duration-200 text-transparent"
	data-slick-dot-inactive="bg-transparent"
	data-slick-dot-active="bg-primary"
></ul>
```