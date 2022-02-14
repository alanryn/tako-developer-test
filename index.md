## Tako Agency Developer Test Solution  

### Instruction

1. Create a “members only” page, that is only accessible to registered + logged in members who are tagged “vip.” In addition, create a link in the navigation menu to the members only page that is only visible to logged in members with the appropriate vip tag.

### Answer

- Create a new template called `page.member.liquid` in the `templates` folder. This will be the template for the Member page.  

- Create a new page called `Member`. Make sure the page uses the `page.mamber.liquid` template ( if you don't see the the template, it is only available in the dropdown list after the theme is published)

- While in the Shopify Admin, add a new menu with a `Member` page link. This menu will relplace the regular menu when a customer is logged-in and tagged as "vip".

- Optional: Add a message in case a non-vip member gets to the member page without being logged in or tagged as "vip":

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

Add a snippet called `non-member-message.liquid` with the message: 

```html
{% raw %}
<p>This is the VIP members area.</p>
{% unless customer %}
<a href="/account/login">Log-in</a> to your account to see if you are a VIP customer.
{% endunless %}
To find out more <a href="/pages/contact-us">Contact Us</a>
{% endraw %}
```
- Create a customer account.  

- Set a 'vip' tag for the customer.  

- Change the navigation in the `header-classic.liquid` section file to check for a vip logged-in customer (remember to change  mobile version -- ``mobile-menu.liquid` and `mobile-menu-loop-liquid` in `snippets` folder):
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

### Instruction  
2. On the “members only” page, create a customizer block inside the theme editor that allows the client to upload an image that will display at the top of the page.  

## Answer
- Add a section called `client-upload-alan-ryan.liquid` Here's the code I added:  

```html
{% raw %}
<div class="member-container">
{% for block in section.blocks %}
  <img src="{{ block.settings.my_image | img_url: '720x' }}" alt="{{ block.settings.my_image.alt | escape }}"/>
{% endfor %}
</div> 

<style>
   .member-container img{
    width: 100%;
    aspect-ratio: 1;
    object-fit: cover;
  }
  .member-container{
    display: grid;
    grid-gap: 2rem;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    margin: 2rem auto; 
    padding: 0 2rem;
  }
</style>

{% schema %}
  {
    "name": "Upload Image",
    "settings": [],
    "blocks": [{
        "type": "image",
        "name": "Upload an image",
         "settings": [
         {
           "type": "image_picker",
           "id": "my_image",
           "label": "Member Image"
         }
       ]
    }]
}
{% endschema %}
{% endraw %}
```
- Now add the section to the `page.member.liquid` file.
- In the theme customizer navigate to the member page. The `image upload` section will be available and it is possible to upload images to a grid.

### Instruction  
3. On any products or variants that are out of stock, show a “contact us” button that goes to the contact us page

### Answer

### Instruction  
4. On variant change/selection, show only images that are associated with that variant (you may need to add some more random images to a product to make this work.)

### Answer


