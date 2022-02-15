## Tako Agency Shopify Developer Test Solution  

## Theme version FLEX - ALAN RYAN - FEBRUARY 22

### Question

1. Create a "members only" page, that is only accessible to registered + logged in members who are tagged “vip.” In addition, create a link in the navigation menu to the members only page that is only visible to logged in members with the appropriate vip tag.

### Answer

- Create a new template called `page.member.liquid` in the `templates` folder. This will be the template for the Member page.  

- Create a new page called `Member Alan Ryan`. Make sure the page uses the `page.mamber.liquid` template (if you don't see the template, it is only available in the dropdown list after the theme is published)

- While in the Shopify Admin, add a new menu called `Member Menu Alan Ryan` with a `Member` page link. This menu will replace the regular menu when a customer is logged-in and tagged as &ldquo;vip&rdquo;.

- Optional: Add a message in case a user gets to the member page without being logged-in and tagged as "vip":

In the `theme.liquid` file add a test to determine if the user is logged-in and a "vip":
{% raw %}
```
{% if template contains 'member' %}
    {% if customer %}
        {% if customer.tags contains 'vip' %}
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

Add a snippet called `non-member-message.liquid` with a no access message: 

```html
{% raw %}
<p>This is the VIP members area.</p>
{% unless customer %}
<a href="/account/login">Log-in</a> to your account to see if you are a VIP customer.
{% endunless %}
To find out more <a href="https://tako-dev-test.myshopify.com/pages/contact">Contact Us</a>
{% endraw %}
```
- Create a customer account.  

- Set a 'vip' tag for the customer.  

- Change the navigation in the `header-classic.liquid` section file to check for a vip logged-in customer (remember to change  mobile version menu files -- `mobile-menu.liquid` and `mobile-menu-loop-liquid` in `snippets` folder):
 {% raw %}
 ```
 {% assign menu-name = "main-menu" %}
 {% if customer %}
        {% if customer.tags contains 'vip' %}
          {% assign menu-name = "member-menu-alan-ryan" %}
        {% endif %}
 {% endif %}
{% for link in linklists[menu-name].links %}
```
{% endraw %}  

Now when a customer is logged in and tagged as "vip" the menu changes to display the Mamber option.

### Question  

2. On the “members only” page, create a customizer block inside the theme editor that allows the client to upload an image that will display at the top of the page.  

### Answer  

- Add a section called `client-upload-alan-ryan.liquid`. Here's the code I added:  

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

### Question 

3. On any products or variants that are out of stock, show a “contact us” button that goes to the contact us page

### Answer

- Edit the `snippets/product.liquid` file. I added the following code at line 287, before the social media icons:

```
{% raw %}
   {% comment %} Start of code to show "Contact Us" link when item is sold out {% endcomment %}
     {% comment %}  Store all the variants as an object {% endcomment %}
    {% capture 'variants' %}       
     {% for variant in product.variants %}
        {{variant.id}}:{{ variant.available | json }}
        {% unless forloop.last %},{% endunless %}           
     {% endfor %}
    {% endcapture %}  
    
     {% comment %} Show and hide the "Contact Us" link {% endcomment %}
      <div class="hideMe">
          <p><a class="contact-btn" href="pages/contact-us">Contact Us</a></p>       
      </div>
    
     {% comment %} Script to loop through object values--setting display to none for true values {% endcomment %}
      <script>
        const currentVariantId = {{ product.selected_or_first_available_variant.id }};
        const data = { {{ variants }} };        
        const variantAvailable = (id) => {
               const hide = document.querySelector('.hideMe')
                   if (data[id]) {
                       hide.style.display = 'none'
                    }
                   else 
                      hide.style.display = 'block'
              }
              variantAvailable(currentVariantId);
      </script>
 {% comment %}  End of code to show "Contact Us" link when item is sold out {% endcomment %}
{% endraw %}
```
- The code creates an object variable to store key value pairs containing `variant.id` and `variant.available`. The js looops through the values and displays or hides the "Contact Us" link depending on the `variant.available` value.  

- I then updated the `assets/app.js.liquid` file (line 552) to call the `variantAvailable` function when the variant changes:

```
{% raw %}
_updateVariantSelection(product, selectedOptions) {
    if (!this._variantSelection) return;
    const variant = getVariantFromSelectedOptions(product.variants, selectedOptions);
    const isNotSelected = selectedOptions.some(option => option === 'not-selected'); // Update master select

    if (variant) {
      this._variantSelection.variant = variant.id;
      
      // added by Alan. Calling variantAvailable function in snippets/product.liquid.
      // Passing the selected variant id
      console.log("id:", variant.id);
      variantAvailable(variant.id);
    } else {
      this._variantSelection.variant = isNotSelected ? 'not-selected' : 'unavailable';
    }
  }

{% endraw %}
```

### Question  
4. On variant change/selection, show only images that are associated with that variant (you may need to add some more random images to a product to make this work.)

### Answer

- I wasn't sure what was required here. I would just hide the thumbnail images for the product and add an image for each of the variants. That way when you change variant you only see the image for that variant. Otherwise you could change the code in the `product__images.liquid` file -- hide the thumbnails if there is a variant image set and show them if there is no unique variant image.

### Changes I made to the theme:

- Added a menu called `Member Menu Alan Ryan`.
- Added a page called `Member Alan Ryan`.
- Created a customer called  `Alan Ryan` with tag of 'vip'.
- Updated the `theme.liquid` to display a message on the member page if customer is not logged-in and tagged as "vip".
- Created `snippets/non-menu-message.liquid` to display message to non-vip customers.
- Created a page template called `page.member.liquid`.
- Created a section file called `client-upload-alan-ryan`.
- Updated `header-classic.liquid` to switch menus on desktop.
- Updated `snippets/mobile-menu.liquid` and `snippets/mobile-menu-loop` to switch menus on mobile.
- Updated `snippets/product.liquid` to display "Contact Us" link for sold out variants.
- Updated `assets/app.js.liquid` to call function on variant change.
