---
title: Custom Addresses
permalink: /reference/custom-attributes/custom-addresses
---

Custom addresses are used to add one or many addresses to edited profile.

## Fields

Each address supports following self-descriptive fields:

| Field name        | Type   | Note                                                                                                                                      |
| ----------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| address           | String | address that has been sent via form - usually original input from user                                                                    |
| latitude          | Float  | latitude part of coordinate used for geo searching                                                                                        |
| longitude         | Float  | longitude part of coordinate used for geo searching                                                                                       |
| formatted_address | String | formatted address that you want to display on your MP. f.e. you can send `address` to google API and get more complete address to display |
| street            | String |
| suburb            | String |
| city              | String |
| state             | String |
| country           | String |
| iso_country_code  | String | 2 letter [ISO code](http://kirste.userpage.fu-berlin.de/diverse/doc/ISO_3166.html)                                                        |
| postcode          | String |

## Defining

CustomAddresses are enabled by adding a CustomAttribute with `attribute_type` set to `address` for example:

```yml
- name: hq_address
  attribute_type: address
- name: correspondence_address
  attribute_type: address
```

## Form configuration

Having this declaration in any of `instance_profile_types/` files will result in 2 CustomAddresses being available in the form configuration, where they can be used for example like this:

```yml
  profiles:
    validation: { presence: true }
    seller:
      validation: { presence: true }
      custom_addresses:
        hq_address:
          validation: { presence: true }
          address:
            validation: { presence: true }
          city:
            validation: {}
          state:
            validation: { presence: true }
          street:
            validation: {}
          country:
            validation: {}
          postcode:
            validation: {}
        correspondence_address:
          address:
            validation: { presence: false }
          city:
            validation: {}
          state:
            validation: {}
          street:
            validation: {}
          country:
            validation: {}
          postcode:
            validation: {}
```

## Displaying, updating

After having this, the way for editing those addresses can be further customized in the markup for the forms:

{% raw %}

```liquid
{% fields_for profiles %}
  {% fields_for seller, form: profiles %}
    {% fields_for custom_addresses, form: seller %}

      <h2> Headquaters address </h2>
      {% fields_for hq_address, form: custom_addresses %}
        {% input address, form: hq_address %}
        {% input city, form: hq_address %}
        {% input street, form: hq_address %}
        {% input state, form: hq_address %}
        {% input country, form: hq_address %}
        {% input postcode, form: hq_address %}
      {% endfields_for %}

      <h2> Correspondence address </h2>
      {% fields_for correspondence_address, form: custom_addresses %}
        {% input address, form: correspondence_address %}
        {% input city, form: correspondence_address %}
        {% input street, form: correspondence_address %}
        {% input state, form: correspondence_address %}
        {% input country, form: correspondence_address %}
        {% input postcode, form: correspondence_address %}
      {% endfields_for %}

    {% endfields_for %}
  {% endfields_for %}
{% endfields_for %}
```

{% endraw %}