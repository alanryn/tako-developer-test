## Tako Agency Developer Test Solution  

### Instructions

1. Create a “members only” page, that is only accessible to registered + logged in members
who are tagged “vip.” In addition, create a link in the navigation menu to the members only page that is only visible to logged in members with the appropriate vip tag.
- Create a new json template called page.member.json. This will be the template for the Member page.
- Create a new page called Member;make sure the page uses the page.mamber.json template ( if you don't see the the template in the dropdown list it is only available after you publish the store)
2. On the “members only” page, create a customizer block inside the theme editor that allows the client to upload an image that will display at the top of the page.

3. On any products or variants that are out of stock, show a “contact us” button that goes to the contact us page

4. On variant change/selection, show only images that are associated with that variant (you may need to add some more random images to a product to make this work.)

```
{% raw %}
```html
{% if template contains 'member' %}
    {% if customer %}
        {% if customer.tags contains 'member' %}
            {{ content_for_layout }}
        {% else %}
            {% render 'non-member-message' %}
        {% endif %}
    {% else %}
            {% render 'non-member-message' %}
    {% endif %}
{% else %}
    {{ content_for_layout }}
{% endif %}
```
{% endraw %}

