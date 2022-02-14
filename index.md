## Tako Agency Developer Test Solution  

### Instructions

1. Create a “members only” page, that is only accessible to registered + logged in members who are tagged “vip.” In addition, create a link in the navigation menu to the members only page that is only visible to logged in members with the appropriate vip tag.

- Create a new json template called `page.member.json` in the `templates` folder. This will be the template for the Member page.  

- Create a new page called `Member`. Make sure the page uses the `page.mamber.json` template ( if you don't see the the template is only available in the dropdown list after the theme is published)

- While in the Shopify Admin, add a new menu with a `Member` page link. This menu will relplace the regular menu when a customer is logged-in and tagged as "vip".

- Optional: Add a message in case non vip member gets to the member page without being logged in or tagged as "vip":

In `theme.liquid` file add a test to determine if the user is logged-in:
{% raw %}
```
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

Add a snippet called `non-member-message.liquid` to apply message: 

```html
{% raw %}
<p>This is the VIP members area.</p>
{% unless customer %}
<a href="/account/login">Log-in</a> to your account to see if you are a VIP customer.
{% endunless %}
To find out more <a href+"/pages/contact-us">Contact Us</a>
 {% endraw %}
```
- Create a customer account.  

- Set a tag of on the account of "vip".  

- Change the navigation in the `header.liquid` section file to check for vip logged-in customer (make change for desktop and mobile versions):
 {% raw %}
 ```
 {% assign menu-name = "main-menu" %}
 {% if customer %}
        {% if customer.tags contains 'vip' %}
          {% assign menu-name = "member-menu" %}
        {% endif %}
 {% endif %}
{% for link in linklists[menu-name].links %}
```
{% endraw %}


2. On the “members only” page, create a customizer block inside the theme editor that allows the client to upload an image that will display at the top of the page.

3. On any products or variants that are out of stock, show a “contact us” button that goes to the contact us page

4. On variant change/selection, show only images that are associated with that variant (you may need to add some more random images to a product to make this work.)



