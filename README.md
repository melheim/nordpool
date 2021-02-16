## nordpool custom component for home assistant

### Usage

Set up the sensor using the webui or use a yaml.

The sensors tries to set some sane default so a minimal setup can be

```
sensor:
  - platform: nordpool
    region: "Kr.sand" # This can be skipped if you want Kr.sand
```



in configuration.yaml

```
nordpool:

sensor:
  - platform: nordpool

    # Should the prices include vat? Default True
    VAT: True

    # What currency the api fetches the prices in
    # this is only need if you want a sensor in a non local currecy
    currency: "EUR"

    # Helper so you can set your "low" price
    # low_price = hour_price < average * low_price_cutoff
    low_price_cutoff: 0.95

    # What power regions your are interested in.
    # Possible values: "DK1", "DK2", "FI", "LT", "LV", "Oslo", "Kr.sand", "Bergen", "Molde", "Tr.heim", "Tromsø", "SE1", "SE2", "SE3","SE4", "SYS", "EE"
    region: "Kr.sand"

    # How many decimals to use in the display of the price
    precision: 3 

    # What the price should be displayed in default
    # Possible values: MWh, kWh and Wh
    # default: kWh
    price_type: kWh

    friendly_name: "Power"

    # This option allows the usage of a template to add a tariff.
    # now() always refers start of the hour of that price.
    # this way we can calculate the correct costs the future and add that to graphs etc.
    # The price result of the tariff expects this additional cost to be in kWh and not cents
    # default {{0.0}}
    additional_costs: ""
      
```

```
# Tariff example
'{% set s = {
    "hourly_fixed_cost": 0.5352,
    "winter_night": 0.265,
    "winter_day": 0.465,
    "summer_day": 0.284,
    "summer_night": 0.246,
    "cert": 0.01
}
%}
{% if now().month >= 5 and now().month <11 %}
    {% if now().hour >=6 and now().hour <23 %}
        {{s.summer_day+s.hourly_fixed_cost+s.cert}}
    {% else %}
        {{s.night+s.hourly_fixed_cost+s.cert}}
    {% endif %}
{% else %}
    {% if now().hour >=6 and now().hour <23 %}
        {{s.winter_day+s.hourly_fixed_cost+s.cert}}
    {%else%}
        {{s.winter_night+s.hourly_fixed_cost+s.cert}}
    {% endif %}
{% endif %}'
```


run the create_template script if you want one sensors for each hour. See the help options with ```python create_template --help``` you can run the script anyhere python is installed. (install the required packages pyyaml and click using `pip install packagename`)
