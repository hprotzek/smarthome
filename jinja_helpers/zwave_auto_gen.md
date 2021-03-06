```

#################################################################
#   @author         :   Mahasri Kalavala
#   @date           :   {{ now().month  ~ '/' ~ now().day ~ '/' ~ now().year }}
#   @package        :   Z-Wave package
#   @description    :   Z-Wave Still and it's configuration stuff
#
#   This entire page is auto generated by the script (link below):
#   https://github.com/skalavala/smarthome/blob/master/jinja_helpers/zwave_auto_gen.md
#################################################################

homeassistant:
  customize:

# ZWave Binary Sensors
{% for zitem in states.zwave -%}
{% for state in states.binary_sensor if zitem.entity_id.split('.')[1] in state.entity_id %}
    {{ state.entity_id }}:
      friendly_name: {{ state.name.replace('Sensor Sensor', 'Sensor') }}
{%- endfor %}
{%- endfor %}

# ZWave Sensors
{% for zitem in states.zwave -%}
{% for state in states.sensor if zitem.entity_id.split('.')[1] in state.entity_id %}
    {{ state.entity_id }}:
      friendly_name: {{ state.name }}
{%- endfor %}
{%- endfor %}

# ZWave Switches
{% for zitem in states.zwave -%}
{% for state in states.switch if zitem.entity_id.split('.')[1] in state.entity_id %}
    {{ state.entity_id }}:
      friendly_name: {{ state.name.replace("Switch Switch", "Switch") }}
{%- endfor %}
{%- endfor %}

zwave:
  usb_path: /dev/ttyACM0

group:

  Motion Sensors:
{% for item in states.binary_sensor if not 'workday' in item.entity_id %}
{%- if loop.first %}    entities:{% elif loop.last %}{% else %}{% endif %}
      - {{ item.entity_id }}{% endfor %}

  Light Levels:
{% for item in states.sensor %}
{%- if loop.first %}    entities:{% elif loop.last %}{% else %}{% endif -%}

{%- if 'luminance' in item.entity_id %}
      - {{ item.entity_id }}{%- endif -%}{% endfor %}

  Humidity Levels:
{% for item in states.sensor if not  'kalavala' in item.entity_id and not  'dark_sky' in item.entity_id%}
{%- if loop.first %}    entities:{% elif loop.last %}{% else %}{% endif -%}

{%- if 'humidity' in item.entity_id %}
      - {{ item.entity_id }}{%- endif -%}{% endfor %}

  Temperature Levels:
{% for item in states.sensor if not  'kalavala' in item.entity_id and not  'dark_sky' in item.entity_id %}
{%- if loop.first %}    entities:{% elif loop.last %}{% else %}{% endif -%}

{%- if 'temperature' in item.entity_id %}
      - {{ item.entity_id }}{%- endif -%}{% endfor %}

{% for item in states.zwave if not 'repeater' in  item.entity_id and not 'stick' in item.entity_id %}
  {{ item.name }}:
    entities:
{%- for x in states.sensor if item.entity_id.split('.')[1] in x.entity_id %}
      - {{ x.entity_id -}}
{% endfor %}
{% endfor %}

  ZWave Devices:
    entities:
{%- for item in states.zwave %}
      - {{ item.entity_id }}
{%- endfor %}

```
