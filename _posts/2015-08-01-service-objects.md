---
layout: post
title: Your Rails Models Are Too Fat
---

Okay Rails developers, we need to have an intervention. Who has heard this mantra before?

>
> **Skinny models, fat controllers**
>

This concept has always baffled me. Typically, what you end up with is a model littered with a multitude of `before_save`, `after_create`, and on-validation-time callbacks with all these crazy side-effects that you may only want performed within the context of a single controller action. The business logic of your application slowing seeps into and infects the models of your Rails application, which are supposed to only be concerned with persistence and querying.

## Keep your dirty business logic out of my models!

What do I mean when I say business logic? Well, it is a pretty loosely used term in the tech industry. For years I struggled with the concept of "business logic" and what it means. In my head, I define it as:

>
> The interactions that happen between your objects within your application
>

Take, for example, purchasing the contents of a shopping cart. You may want to transform that cart into an order object then call out to a few 3rd party APIs for tax/shipping calculation. Once you have the price you may have to create some kind of order invoice to send down to the warehouse where the items purchased are housed then you have to email the user to inform them that their order is being processes. That is a lot of interactions between many different objects and even 3rd party APIs. Where does all this logic live? Does all this purchase-only related logic belong on your order model?

Well, lets say that you do move that logic into the model. Well, over time it is going to get jumbled up with logic for archiving an order, refunding an order, ect. Soon, coders will start using methods written for purchasing that were never meant to be used outside the purchase flow! Over time, a rat nest of methods grow in your model that become less and less concerned with querying and persistence and more concerned with the business logic of your application. All you've done is move a mess that would be in your controller into your model-layer.

<img src="http://i.imgur.com/8xx1fo1.jpg" height='300'/>

## Enter Services

Lets go back to that example of a purchasing the contents of your shopping cart. You created a `purchase` action on your `CartsController` containing everything that you needed to perform a purchase (details omitted for brevity sake)

~~~ruby
...

def purchase
  @cart = current_user.cart
  @order = transform_cart_into_order(@cart)
  @order.calculate_tax!
  @order.calculate_shipping!
  @order.send_order_request_to_warehouse!
  @order.email_invoice_to_user(current_user)!
end
...
~~~

But you received a code-review requesting that you "move all this purchase logic out of the controller." So, like a good coder, you move your purchase logic into the order

~~~ruby
class Order
  has_one :user
  has_one :shipping_address
  has_one :credit_card
  has_many :line_items

  def from_cart(cart)
    (...)
  end

  def purchase
    transform_cart_into_order(@cart)
    calculate_tax!
    calculate_shipping!
    send_order_request_to_warehouse!
    email_invoice_to_user(current_user)!
  end
end
~~~

Now you are one step closer to the problem I have detailed above. Models were created to concern themselves with querying, persistence, and relations between other models in your system. Littering your models with this interaction-based business object is messy. What I propose is that you consider adding a new concept to your rails application called a Service layer. Which contains all this business logic governing the way that your application's models interact with each other.

~~~
|-app/
| |-controllers/
| | |-users_controller.rb
| | |-orders_controller.rb
| |-helpers/
| |-mailers/
| | |-order.rb
| | |-user.rb
| | |-cart.rb
| |-models/
| |-services/
| | |-order/
| | | |-purchase_service.rb
~~~

~~~ruby
class Order::PurchaseService
  attr_accessor :cart, :user, :credit_card

  def initialize(cart, user, credit_card)
    @cart = cart
    @user = user
    @credit_card = credit_card
  end

  def perform
    transform_cart_into_order(@cart)
    calculate_tax!
    calculate_shipping!
    charge_credit_card
    send_order_request_to_warehouse!
    email_invoice_to_user(current_user)!
  end

  ...
end
~~~

Now, all of your purchasing logic and concepts are contained in a single place. This service can also be reused anywhere in your application to have any user purchase any cart with any credit_card so long as those 3 objects are passed into the service.

For bonus credit, checkout the [mutations](https://rubygems.org/gems/mutations) gem that I use to some simple validations and error checking to my service layer.
