---
layout: default
permalink: /appendix/
title: 附录
class: license-types
---

供参考，这是 [choocealicense.rustwiki.org](https://github.com/choosealicense-cn/choosealicense-cn.github.io) 存仓库中描述的全部许可证的表格。

如果您在这里选择许可证，**[请从主页开始](/)**，查看适用于大多数情况的少数几个许可证。

<table border style="font-size: xx-small">
{% assign types = "permissions|conditions|limitations" | split: "|" %}
<tr>
  <th scope="col" style="text-align: center">许可协议</th>
  {% assign seen_tags = '' %}
  {% for type in types %}
    {% assign rules = site.data.rules[type] | sort: "label" %}
    {% for rule_obj in rules %}
      {% if seen_tags contains rule_obj.tag or rule_obj.tag contains '--' %}
        {% continue %}
      {% endif %}
      {% capture seen_tags %}{{ seen_tags | append:rule_obj.tag }}{% endcapture %}
      <th scope="col" style="text-align: center; width:7%"><a href="#{{ rule_obj.tag }}">{{ rule_obj.label }}</a></th>
    {% endfor %}
  {% endfor %}
</tr>
{% assign licenses = site.licenses | sort: "path" %}
{% for license in licenses %}
  <tr style="height: 3em"><th scope="row"><a href="{{ license.id }}">{{ license.title }}</a></th>
  {% assign seen_tags = '' %}
  {% for type in types %}
    {% assign rules = site.data.rules[type] | sort: "label" %}
    {% for rule_obj in rules %}
      {% assign req = rule_obj.tag %}
      {% if seen_tags contains req  or rule_obj.tag contains '--' %}
        {% continue %}
      {% endif %}
      {% capture seen_tags %}{{ seen_tags | append:req }}{% endcapture %}
      {% assign seen_req = false %}
      {% for t in types %}
        {% for r in license[t] %}
          {% if r contains req %}
            <td class="license-{{ t }}" style="text-align:center">
              {% if r contains "--" %}
                {% assign lite = " lite" %}
              {% else %}
                {% assign lite = "" %}
              {% endif %}
              <span class="{{ r | append: lite }}">
                <span class="license-sprite {{ r }}"></span>
              </span>
            </td>
            {% assign seen_req = true %}
          {% endif %}
        {% endfor %}
      {% endfor %}
      {% unless seen_req %}
        <td></td>
      {% endunless %}
    {% endfor %}
  {% endfor %}
  </tr>
{% endfor %}
</table>

## 说明

<p>开源许可证授予公众 <span class="license-permissions"><span class="license-sprite"></span></span> <strong>权限</strong>来使用已许可的作品，而通常版权或其他“知识产权”法律会却禁止这样做。</p>

<p>大多数开源许可证的许可授予都必须遵守 <span class="license-conditions"><span class="license-sprite"></span></span> <strong>条件</strong>。</p>

<p>大多数开源许可证也通常有免除保修和责任的 <span class="license-limitations"><span class="license-sprite"></span></span> <strong>限制</strong>，有时明确将专利或商标从许可授予中排除。</p>

<dl>
{% assign seen_tags = '' %}
{% for type in types %}
  {% assign rules = site.data.rules[type] | sort: "label" %}
  {% for rule_obj in rules %}
    {% assign req = rule_obj.tag %}
    {% if seen_tags contains req %}
      {% continue %}
    {% endif %}
    <dt id="{{ req }}">{{ rule_obj.label }}</dt>
    {% capture seen_tags %}{{ seen_tags | append:req }}{% endcapture %}
    {% for t in types %}
      {% assign rs = site.data.rules[t] | sort: "label" %}
      {% for r in rs %}
        {% if r.tag == req %}
          <dd class="license-{{t}}"><span class="license-sprite"></span> {{ r.description }}</dd>
        {% endif %}
      {% endfor %}
    {% endfor %}
  {% endfor %}
{% endfor %}
</dl>
