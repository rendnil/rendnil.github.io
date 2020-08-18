---
layout: page
title: NYC Restaurant List
---
<br/>
<iframe src="https://www.google.com/maps/d/embed?mid=1K8NS1yfCfDFwpENGEdiBH5K2x2o-ZfPx" width="680" height="600"></iframe>

<table>
  {% for row in site.data.restaurants %}
    {% if forloop.first %}
    <tr>
      {% for pair in row %}
        <th>{{ pair[0] }}</th>
      {% endfor %}
    </tr>
    {% endif %}

    {% tablerow pair in row %}
      {{ pair[1] }}
    {% endtablerow %}
  {% endfor %}
</table>
