
{{ model.name + ":" + method.name }}({% with function=method %}{% include "param_list.rst" %}{% endwith -%})
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

{%- filter indent(width=4) %}
{% if method.is_virtual -%}
:virtual:
{% endif -%}
{% if method.is_abstract -%}
:abstract:
{% endif -%}
{% if method.is_deprecated -%}
:deprecated:
{% endif -%}
{% if method.visibility == "protected" -%}
:protected:
{%- endif %}

{% if method.short_desc -%}
{{ method.short_desc | process_link }}
{% endif %}
{% if method.desc -%}
{{ method.desc | process_link }}
{%- endif %}



{% if method.params -%}
**Parameters:**
	
{%- endif %}

{% for param in method.params -%}
{%- with type=param.type -%}
{%- if param.name == "..." -%}
:param vararg: {{ param.desc|process_link }}
:type vararg: {% include "type.rst" %}
{% else -%}
{{ "	- " + param.name }} :code:`{% include "type.rst" %}` {{ param.desc|process_link }}

{% endif -%}
{% endwith -%}
{%- endfor -%}



{% if method.returns -%}
**Returns:**
	
{%- endif %}

{% for return in method.returns -%}
{% with type=return.type %}
	- :code:`{% include "type.rst" %}` {{ return.desc }}

{% endwith %}
{%- endfor -%}



{%- if method.usage %}
**Usage:**

.. code-block:: lua
    :linenos:

    {{ method.usage|indent(4) }}
{% endif %}

{% if 'show-source' in options %}
.. container:: toggle

    .. literalinclude:: {{ file_path }}
        :language: lua
        :lines: {{ method|start_stop_line(file_path) }}

{% endif %}
{%- endfilter %}