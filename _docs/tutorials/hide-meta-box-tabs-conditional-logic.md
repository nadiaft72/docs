---
title: Hide Tabs with Conditional Logic
---

Conditional Logic works with any DOM elements. That means you can hide your tabs with Meta Box Conditional Logic. This guide show you how to do that.

Because Tabs isn't regular field or meta box so you have to use `rwmb_outside_conditions` filter to tell Conditional Logic add this element. Imagine you have create a meta box like this:

```php
add_filter( 'rwmb_meta_boxes', function($meta_boxes) {
    $meta_boxes[] = array(
        'title'     => __( 'Meta Box Tabs 2', 'rwmb' ),
        'tabs'      => array(
            'bio'      => __( 'Biography', 'rwmb' ),
            'interest' => __( 'Interest', 'rwmb' ),
        ),
        'tab_style' => 'box',
        'fields'    => array(
            array(
                'name' => __( 'Bio', 'rwmb' ),
                'id'   => 'bio',
                'type' => 'textarea',
                'tab'  => 'bio',
            ),
            array(
                'name' => __( 'Interest', 'rwmb' ),
                'id'   => 'interest',
                'type' => 'textarea',
                'tab'  => 'interest',
            ),
        ),
    );

    return $meta_boxes;
});
```

This will generate a meta box with two tabs `Biography` and `Interest`. And here is the html output:

```html
<ul class="rwmb-tab-nav">
    <li class="rwmb-tab-bio rwmb-tab-active" data-panel="bio"><a href="#">Biography</a></li>
    <li class="rwmb-tab-interest" data-panel="interest"><a href="#">Interest</a></li>
</ul>
```

If you want to hide a tab, just add its class to `rwmb_outside_conditions` filter. For example, if I want to hide `Interest` tab when post format is `aside`. Simply add these lines:

```php
add_filter( 'rwmb_outside_conditions', function( $conditions ) {
    $conditions['.rwmb-tab-interest'] = array(
        'hidden' => array('post_format', 'aside')
    );
    return $conditions;
} );
```

This will tell Conditional Logic hide `.rwmb-tab-interest` selector when `post_format` is `aside`

### Hide the first tab

In case you want to hide the first tab, you have to hide its tab pane also and show the next tab pane, this example shows you how to do that:

```php
add_filter( 'rwmb_outside_conditions', function( $conditions ) {
    $conditions['.rwmb-tab-bio, .rwmb-tab-panel-bio'] = array(
        'hidden' => array('post_format', 'aside')
    );
    $conditions['.rwmb-tab-panel-interest'] = array(
        'visible' => array('post_format', 'aside')
    );
    return $conditions;
} );
```