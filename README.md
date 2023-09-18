# Twig Attribute Parser

Takes an array of attributes to be converted into HTML formatted attributes ready for use in an HTML element.

The optional `tag` attribute will also generate the required HTML element and is useful for contextually switching elements within Twig views.

Output will always begin with an empty space to avoid unwanted spaces if no attributes are passed to the markup.



## Example

```twig
{%
    set arrayToProcess = ['slim': 'shady', 'marshall': 'mathers', 'eminem': true, 'class': 'wrapper']
%}

<span{{ attributes(arrayToProcess) }} />

{# output: <span slim="shady" marshall="mathers" eminem class="wrapper" /> #}
```

## Boolean attributes

Boolean attribute values will output the key only. The `disabled` boolean attribute behavior is contextual based on the presence and type of an optional `tag` (see below).

```twig
{%
    set arrayToProcess = ['foo': true]
%}

<span{{ attributes(arrayToProcess) }} />

{# output: <span foo> #}
```

## Disabled attribute

The `disabled` attribute is only valid on specific elements, if you attempt to pass a boolean `disabled` attribute without a valid `tag` or to a tag which doesn’t support the disabled attribute, it will output a disabled class instead.

```twig
{%
    set arrayToProcess = ['disbled': true]
%}

<span{{ attributes(arrayToProcess) }} />

{# output: <span class="disabled" /> #}
```

If a `class` attribute is also present, the class list will be merged appropriately. If the `class` list also contains `is-disabled` it will not be duplicated.

```twig
{%
    set arrayToProcess = ['class': 'foo', 'disbled': true]
%}

<span{{ attributes(arrayToProcess) }} />

{# output: <span class="foo disabled" /> #}
```

If `tag` is `button`, `fieldset`, `input`, `option`, `select`, `textarea` or `div`, it will output the disabled attribute.

```twig
{%
    set arrayToProcess = ['disbled': true]
%}

{# note the absence of the element here, we let the attribute parser output it #}
<{{ attributes(arrayToProcess, {'tag': ('button')}) }} />

{# output: <button disabled /> #}
```

If you want the output to be `disabled class="disabled"` you will need to pass both attributes in your array.

If `tag` is `a`, it will add the `aria-disabled` attribute instead as an opinionated accessibility choice.

```twig
{%
    set arrayToProcess = ['disbled': true]
%}

<{{ attributes(arrayToProcess, {'tag': ('a')}) }} />

{# output: <a aria-disabled="true" /> #}
```

## HTML entities

HTML entities will be properly quoted to suit HTML markup, this allows embedded HTML to be passed within attributes for later rendering, for example in tooltops.

```twig
{%
    set arrayToProcess = ['foo': '<span>bar</span> <blink>baz</blink>']
%}

<span{{ attributes(arrayToProcess) }} />

{# output: <span foo="&lt;span&gt;bar&lt;/span&gt; &lt;blink&gt;baz&lt;/blink&gt;" /> #}
```

## Empty values

Empty values will be removed, if you want to display only the key, use a boolean instead.

```twig
{%
    set arrayToProcess = ['foo': '']
%}

<span{{ attributes(arrayToProcess) }} />

{# output: <span /> #}
```

## Nested/multidimensional arrays

Not supported.

