---
id: 48
title: How to add wishlist to shopify
date: 2015-07-09T02:21:44+00:00
author: Ashutosh Chaudhary
layout: post
permalink: /how-to-add-wishlist-to-shopify/
dsq_thread_id:
  - 3917316017
views:
  - 325
sociallikes:
  - 1
categories:
  - Shopify
tags:
  - shopify
  - theme development
---
Shopify does not provide “add wishlist” feature natively. It becomes an obvious need for theme developers. So I will help you how to add wishlist to shopify.  [This Github Repo] is all you need for a minimal effort development of this feature. So, I am going to tell how can you use the repo files and have the wishlist functionality to your shopify store. 

**STEP 1**: Download the repo or clone it. you will get 3 files:

-   wish-list.js
-   wishlist-page.liquid
-   wishlist-product.liquid

**STEP 2**: Go to your shopify store’s admin panel. Navigate to your theme (http://your-store-url.com/admin/themes) and choose “Edit HTML/CSS” from the dropdown of active theme. 

**STEP 3**: Upload wish-list.js in “assets” directory. [<img src="http://codeasashu.tk/blog/wp-content/uploads/2015/07/add-assets-1024x318.jpg" alt="add-assets" class="aligncenter wp-image-31 size-large" width="1024" height="318" />] Now you have to create wishlist-page.liquid and wishlist-product.liquid in snippets (just above “assets”). Create both the files and copy the contents of the same files you downloaded. [<img src="http://codeasashu.tk/blog/wp-content/uploads/2015/07/wishlist.jpg" alt="wishlist" class="aligncenter wp-image-32 size-full" width="969" height="273" />] Now that you’ve uploaded the 3 files to your shopify, you can forward to next step. 

**STEP 4**: Now let’s get the things running. So you need 2 things:

1.  Adding a wishlist button to product pages
2.  Making a page where customers can view their wishlist

So, open up product.liquid. What I’m going to do next is very important. So, make sure you do it carefully not making any mistake.

Include the “wishlist-product.liquid” to get the “Add to wishlist” button. So include it as:
 
{% highlight ruby %}
{% raw %}
{% include 'wishlist-product' %}
{% endraw %} 
{% endhighlight %}

You can include it anywhere in product.liquid, but within “Add to cart” form. **Make sure it is outside “Add to cart” form or it won’t work**. So, for a minimal example, it will go like:

{% highlight ruby %}
{% raw %} 
<form action="/cart/add" method="post" enctype="multipart/form-data">
<div id="product" itemscope itemtype="http://schema.org/Product">
  <meta itemprop="url" content="{{ shop.url }}{{ product.url }}" />
  <meta itemprop="image" content="{{ product | img_url: 'grande' }}" />
  {% if product.images.size == 0 %}
    <div id="images">
      <div id="main_image"><img src="{{ '' | img_url: 'grande' }}"></div>
      <div id="thumbnails">
      <div class="thumbs"><img src="{{ '' | img_url: 'grande' }}"></div>
      <div class="thumbs"><img src="{{ '' | img_url: 'grande' }}"></div>
      <div class="thumbs"><img src="{{ '' | img_url: 'grande' }}"></div>
      </div><!-- #thumbnails -->
    </div><!-- #images -->
  {% else %}

  {% assign featured_image = product.selected_or_first_available_variant.featured_image | default: product.featured_image %}

<div id="images">
<div id="main_image"><img class="showlight" src="{{ featured_image | img_url: 'grande' }}"></div>
  <div id="thumbnails">
  {% if product.images.size > 1 %}
        {% for image in product.images %}
        {% if forloop.index <= 3 %} 
        <div class="thumbs"><img src="{{ image | img_url: 'small' }}"></div>
        {% endif %}
        {% endfor %}
  {% else %}
    <div class="thumbs"><img src="{{ '' | img_url: 'grande' }}"></div>
  {% endif %}
  </div>
  {% endif %}
  </div>

<div id="info">
<div id="name">{{ product.title }}</div>
  {% assign variant = product.selected_or_first_available_variant %}
          {% if product.compare_at_price > product.price %}
          <div id="price2">{ variant.price | money }}</div>
          <s class="product-compare-price">{{ variant.compare_at_price | money }}</s>
          {% else %}
          <div id="price2">{{ variant.price | money }}</div>
          {% endif %}
<div id="description">{{ product.description }}</div>
  <div id="product-variants">
        <select id="product-select" name="id">
        {% for variant in product.variants %}
          <option{% if variant == product.selected_or_first_available_variant %} selected{% endif %} value="{{ variant.id }}">
          {{ variant.title }} - {{ variant.price | money }}
          </option>
        {% endfor %}
        </select>
      </div>     
<div id="add"><input type="submit" name="add" id="addtocart" class="primary button" value="{{ add_to_cart | escape }}"></div>
</form>

{% include 'wishlist-product' %} <!-- Notice this is outside the "Add to cart" form -->
</div>
{{ 'wish-list.js' | asset_url | script_tag }} <!-- Placed at bottom of product.liquid -->
{% endraw %}
{% endhighlight %}

If you save the file, you can see a “Add to wishlist” button on product page. Remember, you must be logged-in to make the button work. We’ve completed most of the hard part. The only thing remaining is to view the items of your wishlist.  For that, see next step. 

**STEP 5**:  Creating a page to view wishlist items- Since there might be many pages already in your store, you should design a separate page for wishlist. Shopify provides it in much convenient way. Basic idea to get a unique page to display some unique content is to have a separate template for that page, then selecting that template in the unique page you create. Since you want wishlist page to show only wishlist items and no other bullshit, here is what you’d do:

1.  Create a template for wishlist. Go to “Templates” - “Add a new template”. Select “page” from the dropdown and enter “wishlist” into textbox next to it. [<img src="http://codeasashu.tk/blog/wp-content/uploads/2015/07/wishlist-page.jpg" alt="wishlist-page" class="aligncenter wp-image-34 size-full" width="1018" height="281" />]
2.  It will create a blank “page.wishlist.liquid” file. Enter following contents to it:

{% raw %} 

    ```
    {% include 'wishlist-page' %}
    {{ 'wish-list.js' | asset_url | script_tag }}
    ```

{% endraw %}
Note that the “wish-list.js” acts as a controller to wishlist. Removing it will disable the functionality.

3.  Create a page from “http://your-store-url.com/admin/pages” . Enter Title “Wishlist” and scroll down. You can see a dropdown at bottom, in the “template” section. See the screenshot below[<img src="http://codeasashu.tk/blog/wp-content/uploads/2015/07/page-1024x521.jpg" alt="page" class="aligncenter wp-image-35 size-large" width="1024" height="521" />]
4.  Select “page.wishlist” from the dropdown. It is the same template you just create in previous steps. If you are not seeing any “template” section or dropdown like this, this means you have only “page.liquid” in “template” directory. Create “page.wishlist.liquid” as explained.


Now that you have this page where you can view your wishlist, you have completed all the steps. Try logging in and add a product to wishlist. Go to the wishlist page you created and it will show your wishlist items.

Please do comment below if you need any help.

  [<img src="/static/img/_posts/wishlist-page.jpg" alt="wishlist-page" class="aligncenter wp-image-34 size-full" width="1018" height="281" />]: /static/img/_posts/wishlist-page.jpg
  [<img src="/static/img/_posts/page-1024x521.jpg" alt="page" class="aligncenter wp-image-35 size-large" width="1024" height="521" />]: /static/img/_posts/page.jpg

  [This Github Repo]: https://github.com/jimlakey/Shopify-Wish-List "Wishlist-Shopify"
  [<img src="/static/img/_posts/add-assets-1024x318.jpg" alt="add-assets" class="aligncenter wp-image-31 size-large" width="1024" height="318" />]: /static/img/_posts/add-assets.jpg
  [<img src="/static/img/_posts/wishlist.jpg" alt="wishlist" class="aligncenter wp-image-32 size-full" width="969" height="273" />]: /static/img/_posts/wishlist.jpg