---
layout: article
title: "My papers"
date: 2014-09-25T13:57:25-04:00
modified:
excerpt:
tags: []
image:
  feature:
  teaser:
  thumb:
toc: false
share: false
---

Below is a list of my first authored papers, followed by papers I co-authored. Clicking on
 one of my first authored papers will take you to another page that briefly summarises what
 the paper was about.

## First authored papers

{% assign count = '0' %}
{% assign idx = '0' %}
{% for post in site.categories.papers reversed %}
		{% capture count %}{{ count | plus: '1' }}{% endcapture %}
		{% if post.url == page.url %}
			{% capture idx %}{{count}}{% endcapture %}
		{% endif %}
{% endfor %}

<ul class="unstyled-list">
{% assign count = '0' %}
{% for post in site.categories.papers %}
{% capture count %}{{ count | plus: '1' }}{% endcapture %}
	<li>
		<a href="{{ site.url }}{{ post.url }}">
			<h5>{{ count }}. {% if post.short-title %}{{ post.short-title }}{% else %}{{ post.title | remove: 'Mastering Paper by 53: ' }}{% endif %}</h5>
			<img src="{{ site.url }}/images/{{ post.image.thumb }}" class="preview" alt="preview image">
			<p><i> {{ post.description }}</i> </p>
		</a>
	</li>
{% endfor %}
</ul>

## Other peer-reviewed publications

*Falconer, C., Park, M., Croker, H., Skow, A., Black, JA., Saxena, S., […] Kinra, S.*
(2014).
**The benefits and harms of providing parents with weight feedback as part of the national
child measurement programme: a prospective cohort study**
BMC Public Health, 14(1), 549. http://www.biomedcentral.com/1471-2458/14/549

*Vos, T., Flaxman, A. D., Naghavi, M., Lozano, R., Michaud, C., Ezzati, M., […] Aboyans, V.*
(2013).
**Years lived with disability (YLDs) for 1160 sequelae of
289 diseases and injuries 1990-2010: a systematic analysis for the Global Burden of
Disease Study 2010.** Lancet, 380(9859), 2163–96. doi:10.1016/S0140-6736(12)61729-2

*Murray, C. J. L., Vos, T., Lozano, R., Naghavi, M., Flaxman, A. D., Michaud, C., […] Abdalla, S.*
(2013).
**Disability-adjusted life years (DALYs) for 291 diseases and injuries in 21 regions,
1990-2010: a systematic analysis for the Global Burden of Disease Study 2010.**
Lancet, 380(9859), 2197–223. doi:10.1016/S0140-6736(12)61689-4

*Falconer, C. L., Skow, A., Black, J. A., Croker, H., Sovio, U., Kessel, A., … Kinra, S.*
(2012).
**The majority of parents of overweight and very overweight children underestimate
their child’s weight status and weight-related health risk.**
Archives of Disease in Childhood, 97(Suppl 1),
A181–A181. doi:10.1136/archdischild-2012-301885.424

*Falconer, Catherine L, Park, M., Skow, A., Black, J. A., Sovio, U., Saxena, S., … Kinra, S.*
(2012).
**Scoping the impact of the national child measurement programme feedback on the
child obesity pathway: study protocol.**
BMC public health, 12(1), 783. doi:10.1186/1471-2458-12-783

*Derrett, S., Black, J. A., & Herbison, G. P.*
(2009).
**Outcome after injury-a systematic literature search of studies using the EQ-5D.**
The Journal of trauma, 67(4), 883–90. doi:10.1097/TA.0b013e3181ae6409
